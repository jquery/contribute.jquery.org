<script>{
	"title": "jQuery Core Threat Model",
	"toc": true
}</script>

Built using the [Threat Modeling Template for Libraries](https://gist.github.com/computersarebad/05d5ccbf13250489ea55a240b64c08d0). jQuery is a library, so that template is the right fit — it adds categories (build/publish pipeline, dependency interactions, API-as-attack-surface) that a generic app-level template misses.

A guiding principle threaded throughout this document comes from a jQuery maintainer:

> "jQuery doesn't sanitize user input, so it shouldn't have security issues, right? Not really — an issue we fixed in 3.5.0 was a case where the user has already sanitized HTML before passing it to jQuery, and then jQuery was transforming it to one that could still run an embedded script."

In other words: **a library can re-introduce vulnerabilities into input that was already safe.** Mutation, normalization, and "helpful" rewriting are all places where this happens. This isn't hypothetical for jQuery — CVE-2020-11022 and CVE-2020-11023 are exactly that failure mode. The threat model below treats "making sanitized input unsafe again" as a first-class threat category, not an afterthought.

---

## 1. Library Overview

- **Library Name:** jQuery
- **Brief Description:** A JavaScript library that simplifies HTML document traversal, DOM manipulation, event handling, animation, and Ajax. Its selector engine and chainable API made cross-browser DOM work tractable in the era before modern DOM APIs stabilized. It remains one of the most widely deployed client-side libraries on the web.
- **Key Public APIs/Functions (security-relevant subset):**
  - **DOM injection sinks:** `.html()`, `.append()`, `.prepend()`, `.after()`, `.before()`, `.replaceWith()`, `.wrap()`, `.wrapInner()`, `.wrapAll()`, and the `jQuery(htmlString)` / `$(htmlString)` constructor form.
  - **Object manipulation:** `jQuery.extend()`, `jQuery.extend(true, …)` (deep extend), `jQuery.parseHTML()`.
  - **Network:** `jQuery.ajax()`, `jQuery.get()`, `jQuery.post()`, `jQuery.getJSON()`, `jQuery.getScript()`, JSONP via `jQuery.ajax({ dataType: "jsonp" })`.
  - **Internal parsing/normalization:** `jQuery.htmlPrefilter()` (an identity function since 3.5.0), `jQuery.buildFragment()`, and the selector engine in `src/selector/` (most selection now goes through native `querySelectorAll`).
  - **Event system:** `.on()`, `.off()`, `.trigger()`, delegated event handlers.

## 2. Define Scope

This threat model covers the **core jQuery library itself** — not jQuery UI, jQuery Mobile, or individual plugins. Within core, the focus is on the components with the longest track record of security-relevant findings and the broadest blast radius:

1. **The HTML-accepting API surface** (`.html()`, `.append()`, `$(htmlString)`, etc.) and the internal HTML parsing/normalization pipeline that feeds them.
2. **`jQuery.extend()`**, particularly the deep-merge form, due to its history with prototype pollution.
3. **The Ajax subsystem**, particularly response-type handling and JSONP.
4. **The build, release, and distribution pipeline** (npm, the jQuery CDN, GitHub release artifacts).

Out of scope: plugins, jQuery UI, and application-level misuse that doesn't involve a jQuery transformation of the input.

## 3. Conceptual System Diagram

```
          ┌──────────────────────────────────────────────────────────┐
          │                    Caller Application                     │
          │                                                            │
          │   (developer code — may or may not sanitize before passing)│
          └─────────────────────────┬────────────────────────────────┘
                                    │   HTML strings, objects, URLs,
                                    │   AJAX options, selectors
                                    ▼
          ══════════════════════ TRUST BOUNDARY ══════════════════════
                    (jQuery implicitly trusts its caller;
                     the caller implicitly trusts jQuery's output)
                                    │
          ┌─────────────────────────▼────────────────────────────────┐
          │                         jQuery                             │
          │                                                            │
          │   ┌──────────────┐  ┌──────────────┐  ┌─────────────────┐  │
          │   │  DOM / HTML  │  │ Object utils │  │   Ajax / JSONP   │  │
          │   │   pipeline   │  │   (extend)   │  │                  │  │
          │   │ (htmlPrefilter│  │              │  │                  │  │
          │   │  buildFrag., │  └──────────────┘  └─────────────────┘  │
          │   │  selector)   │                                          │
          │   └──────┬───────┘                                          │
          └──────────┼────────────────────────────────────────────────┘
                     │                    │                    │
          ══════════ TRUST BOUNDARY ═══════════════════════════════════
                     │                    │                    │
                     ▼                    ▼                    ▼
               ┌─────────┐          ┌──────────┐        ┌───────────┐
               │   DOM   │          │ Host page│        │  Network  │
               │ (script │          │  globals │        │  (remote  │
               │execution│          │(Object.  │        │  servers, │
               │context) │          │prototype)│        │   CDN)    │
               └─────────┘          └──────────┘        └───────────┘

          Supply chain (orthogonal):
          Maintainer → GitHub → CI/CD → npm + code.jquery.com CDN → consumer
```

**Key trust assumptions worth being explicit about:**

- jQuery **does not** sanitize HTML. Callers are responsible for that.
- But jQuery **does transform** HTML (historically via `htmlPrefilter`) before inserting it. That transformation is where the "sanitized input becomes unsafe again" problem lives.
- jQuery has no runtime dependencies; the trust question is confined to the build pipeline.
- The caller implicitly trusts that output from jQuery operations is semantically equivalent to input in security-relevant ways. That assumption has been wrong.

## 4. Identify Assets

- **Integrity of DOM operations.** The single most valuable property: what goes into an injection method must produce a DOM that is semantically equivalent to what the caller intended. Breaking this equivalence is the root cause of CVE-2020-11022/11023.
- **Integrity of the host page's JavaScript environment.** Specifically, `Object.prototype` must not be modifiable through normal use of `jQuery.extend()`. CVE-2019-11358 broke this asset.
- **Availability/performance of the host page.** jQuery runs in the browser's main thread; a selector parse with catastrophic backtracking, a deeply nested `extend`, or an infinite animation loop degrades or blocks the consuming application.
- **Confidentiality of cross-origin data.** Improper handling of cross-origin Ajax responses (CVE-2015-9251, the `dataType`-missing-then-`text/javascript` case) can cause attacker-controlled script to execute in the victim origin.
- **Integrity of the distributed artifact.** The file served from `code.jquery.com`, from npm as `jquery`, and from major CDNs must be exactly what the maintainers released. Supply-chain compromise is the highest-impact, lowest-likelihood threat.
- **Reputation of the jQuery project.** Every CVE filed against jQuery is amplified by its ubiquity.
- **Maintainer attention.** jQuery is maintained by a small group of people. Every hour spent reading a low-quality security report is an hour not spent on regression tests, the release pipeline, or real vulnerabilities. Attention is a finite resource and is itself under threat — primarily from automated / AI-generated reports submitted by people who do not understand their own submissions. This is the same class of asset as "availability of the maintainer team," framed as a security property rather than a capacity one.

## 5. Identify Threats

Applying STRIDE to each of the in-scope components, with specific historical examples where they exist.

### 5a. HTML-accepting API surface (`.html()`, `.append()`, `$(htmlString)`, etc.)

| STRIDE | Threat |
| --- | --- |
| **S — Spoofing** | A caller cannot "spoof" in the identity sense, but malicious HTML can impersonate benign markup — e.g., HTML that passes a sanitizer's filter but, after jQuery's transformation, parses as a different tree containing an executable context. This is the core issue the jQuery maintainer flagged. |
| **T — Tampering** | This is the dominant threat for jQuery. The library's internal HTML normalization can tamper with the caller's input. Historical examples: CVE-2020-11022 (the `htmlPrefilter` regex that rewrote self-closing tags like `<style/>` into `<style></style>`, causing later content to escape its intended parent), and CVE-2020-11023 (the same class of bug specifically triggered by `<option>` elements). In both cases the input was sanitized; jQuery's transformation reintroduced a script-executing context. |
| **R — Repudiation** | Low relevance. jQuery does not log actions. |
| **I — Information Disclosure** | Errors thrown from the parser historically expose very little, but selector engine errors could reveal details about the caller's DOM structure to a shared error handler. Minor. |
| **D — Denial of Service** | A pathological selector or deeply nested HTML string fed to the parser can consume CPU / stack. Most selection delegates to native `querySelectorAll`, but the fallback path in `src/selector/` still parses selectors with regexes, where crafted input could cause catastrophic backtracking. |
| **E — Elevation of Privilege** | Not directly — but XSS via tampered HTML is the browser equivalent of EoP: attacker-controlled code runs in the victim origin with the victim's cookies. |

A related caller footgun belongs here rather than with selectors: an untrusted string passed to `$()` that begins with `<` is interpreted as HTML, not as a selector. `$('<img src=x onerror=…>')` constructs (but does not inject) a node with the handler attached, and any later `.appendTo()` fires it. Callers who only intended to select are then one chained call away from executing attacker markup.

### 5b. `jQuery.extend()`, especially deep extend

| STRIDE | Threat |
| --- | --- |
| **S — Spoofing** | An attacker-controlled source object can set a `__proto__` property that, after being merged by `extend(true, target, source)`, causes property lookups on *every* object in the host page to return attacker values. This is a form of identity spoofing at the object level. |
| **T — Tampering** | CVE-2019-11358: `jQuery.extend(true, {}, userInput)` where `userInput` contained `{"__proto__": {...}}` polluted `Object.prototype` itself. Any code anywhere in the page that later read an "unset" property would get the attacker's value. Fixed in 3.4.0 by skipping `__proto__` during merge. |
| **R — Repudiation** | N/A. |
| **I — Information Disclosure** | A polluted prototype can cause objects to leak attacker-controlled defaults into logs, network payloads, or templating output. Indirect but real. |
| **D — Denial of Service** | Extremely deep nested objects passed to deep `extend` consume stack/CPU. |
| **E — Elevation of Privilege** | Prototype pollution is a classic gadget for EoP: it can flip security-relevant flags (`isAdmin`, `authenticated`) on objects the application later trusts. |

### 5c. Ajax subsystem

| STRIDE | Threat |
| --- | --- |
| **S — Spoofing** | JSONP by design allows the responding server to execute script in the caller's origin; if a caller uses `dataType: "jsonp"` against an attacker-controlled or compromised endpoint, the attacker can impersonate legitimate responses. This is a design property, not a bug, but worth modeling. |
| **T — Tampering** | Network-level tampering is out of scope for the library (use HTTPS), but **response-type tampering is in scope**: CVE-2015-9251 — if a caller didn't specify `dataType`, jQuery would inspect the response's `Content-Type`, and a `text/javascript` response from a cross-origin server would be passed to `globalEval`. An attacker who could induce a cross-origin request to a server they controlled got script execution. Fixed by no longer auto-executing cross-origin script responses without explicit opt-in. |
| **R — Repudiation** | N/A at the library level. |
| **I — Information Disclosure** | Default inclusion of cookies on same-origin Ajax (expected), and headers like `X-Requested-With` aiding CSRF defenses. Settings like `crossDomain: true` combined with credentials can leak data; this is caller responsibility but the defaults matter. |
| **D — Denial of Service** | Not a major library-level concern. |
| **E — Elevation of Privilege** | Same as tampering — script execution in the victim origin is the browser-layer EoP. |

### 5d. Build, release, and distribution pipeline

This is the highest-impact, lowest-likelihood category. A compromised jQuery release would affect a meaningful fraction of the web.

| STRIDE | Threat |
| --- | --- |
| **S — Spoofing** | An attacker spoofing a maintainer identity (compromised GitHub account, stolen npm token, compromised CI credentials) and publishing a malicious release as "official." |
| **T — Tampering** | Tampering with the build artifact between `git tag` and the file ultimately served from `code.jquery.com` or npm. This includes compromised build dependencies, malicious CI steps, or CDN-origin intrusion. The canonical mitigation — Subresource Integrity hashes — has to be published by the jQuery project and pinned by consumers. |
| **R — Repudiation** | If a release were tampered, maintainers need to be able to prove what they did and did not ship. Signed releases and reproducible builds matter here. |
| **I — Information Disclosure** | CI logs, cached npm tokens, or GitHub Actions secrets leaking during a build. |
| **D — Denial of Service** | CDN outage or npm registry outage blocking consumers from updating. |
| **E — Elevation of Privilege** | A CI compromise that grants an attacker the ability to publish future releases — the worst-case pivot. |

### 5e. The report intake surface (maintainer attention)

Threats to the project itself as a sociotechnical system, as distinct from threats to the library's code. The STRIDE framing is a little strained here but each row is a real failure mode currently observable.

| STRIDE | Threat |
| --- | --- |
| **S — Spoofing** | Automated agents filing reports under names that look like real researchers, or credit-farming accounts using LLM output to claim credibility they haven't earned. The reporter's identity signals the report's quality less reliably than it used to. |
| **T — Tampering** | Reports that cite jQuery APIs, files, or line numbers that don't exist — fabrications introduced by the generating model and submitted without verification. Reading them wastes time, and in the worst case a maintainer merges a "fix" for a non-existent problem. |
| **I — Information Disclosure** | A real vulnerability report arriving in the same channel as a flood of slop may be dismissed by pattern-match. The signal is drowned in noise. |
| **D — Denial of Service** | **The core threat.** High-volume, low-cost generation of plausible-sounding security reports consumes a finite maintainer attention budget. Every triage cycle spent on slop is one not spent on the regression corpus, the release pipeline, or a real finding. |
| **E — Elevation of Privilege** | Not directly, but adjacent: a slop report that accidentally describes a real issue could gain its submitter undeserved credit, standing, or access (bug-bounty payouts where applicable, GHSA credit, researcher reputation). |

## 6. Mitigations / Countermeasures

| Threat | Proposed Mitigation |
| --- | --- |
| HTML pipeline tampers with already-sanitized input (CVE-2020-11022/11023 class) | Make internal normalization an **identity function wherever possible**. This is what 3.5.0 did: `htmlPrefilter` was turned into `html => html`. **General rule: never mutate input in ways that change its parse tree.** For any future "helpful" transformation, require evidence that it is semantics-preserving across every parser that might see the output. |
| New HTML-affecting changes regressing the above | Maintain a regression corpus of known-tricky inputs (self-closing tags in every element context, `<option>` inside `<select>`, `<template>`, foreign-content elements, HTML-in-SVG, etc.). Run it on every PR. Coordinate with the DOMPurify project on shared test cases. |
| Prototype pollution via `jQuery.extend` | Already mitigated in 3.4.0 by skipping `__proto__`. Keep the skip in place; add explicit tests that `extend(true, {}, JSON.parse('{"__proto__":{"x":1}}')).x` remains `undefined` on a fresh object. Consider also guarding against `constructor.prototype` chains. Document that deep-extending untrusted input is risky even with current mitigations. |
| Cross-origin Ajax auto-executing `text/javascript` (CVE-2015-9251 class) | Fixed: cross-origin requests no longer auto-execute script responses without explicit `dataType: "script"` opt-in. Keep the restrictive default. Audit any future code path that inspects server-supplied `Content-Type` to decide how to handle the body — content-type sniffing is a recurring vulnerability pattern. |
| `$()` constructor treating `<…` strings as HTML | Document the footgun prominently; recommend `jQuery.parseHTML()` when explicit parsing is wanted; consider whether a future major version could require explicit opt-in for HTML interpretation in the `$()` constructor. |
| ReDoS in selector/HTML parsing | Audit regexes for catastrophic backtracking; prefer parser state machines over regex-heavy paths; add fuzz testing (jQuery has used fuzz testing historically — continue and expand). |
| Supply-chain tampering (build/publish) | Enforce 2FA on all maintainer npm and GitHub accounts. Use npm provenance / Sigstore for signed releases. Publish SRI hashes alongside every CDN release and recommend them in documentation. Pin CI dependencies. Minimize the set of secrets available to any single CI job. Maintain a reproducible build. |
| Maintainer identity spoofing | Require signed commits for release tags; hardware-backed keys for publishing. |
| CI compromise | Scope CI tokens to the minimum needed; rotate regularly; require human review for release workflows; log and alert on privileged CI actions. |
| Caller unaware jQuery's HTML methods are script sinks | Documentation: clearly label `.html()`, `.append()`, `.prepend()`, `.after()`, `.before()`, `.replaceWith()`, `.wrap*()`, and `$(htmlString)` as equivalent to `innerHTML` for security purposes, and recommend DOMPurify (without `SAFE_FOR_JQUERY` on 3.5+) for sanitizing untrusted HTML. |
| Caller's sanitizer and jQuery disagree on parse | Publish known interactions with major sanitizers; coordinate disclosure when a new parser quirk is found. The DOMPurify `SAFE_FOR_JQUERY` mode existed precisely because the two libraries' models disagreed — this kind of coordination is a mitigation in itself. |
| DoS on maintainer attention via automated / AI-generated reports | **Three layers, each cheaper than the last.** (1) Policy: a published report-quality bar in CONTRIBUTING.md and SECURITY.md stating what a report must contain and that automated submissions will be closed. (2) Process: a Phase 0 "slop filter" in the IR runbook that closes reports failing the bar in minutes, with a canned decline template. (3) Enforcement: close with a link to the policy, ban repeat submitters, withdraw credit where it was given. The author-engagement test (reporter must answer follow-up questions in their own words) is the single highest-signal filter and should be invoked whenever doubt remains. |

## 7. Risk Ranking

| Threat | Likelihood | Impact | Risk |
| --- | --- | --- | --- |
| HTML pipeline re-introduces XSS in sanitized input | Medium — the parser/sanitizer landscape keeps evolving, and new edge cases are discovered periodically | Very High — affects every consumer doing sanitize-then-inject | **High** |
| Prototype pollution via new `extend`-like code paths | Low (current mitigations are solid) | High — widely exploitable gadget | **Medium** |
| New Ajax auto-execution quirk | Low | High | **Medium** |
| Supply-chain compromise of release pipeline | Low | Catastrophic — affects a double-digit percentage of the web | **High** (because of impact, not likelihood) |
| ReDoS in selector/HTML parsing | Medium | Low–Medium (browser tab becomes unresponsive) | **Low–Medium** |
| Caller footgun: `$('<img onerror>')` | High (prevalence of misuse) | High per incident | **High** — but the mitigation is documentation and education, not code |
| DoS on maintainer attention via automated reports | High (ongoing and growing) | Medium — slow, cumulative; a real report missed in the noise could be High | **High** — primary mitigation is policy and triage process, not code |

## 8. Next Steps & Review

1. **Prioritize the two High-risk items:**
   - Keep the HTML pipeline as close to an identity function as possible. Resist any PR that adds "helpful" HTML rewriting without a semantics-preserving proof.
   - Treat the release pipeline as a production security system. Signed, provenance-attested, reproducible builds.
2. **Regression corpus.** Turn the PoCs from CVE-2020-11022/11023 (and any future equivalents) into a permanent test fixture. Every HTML-handling change must pass.
3. **Coordinate with sanitizer authors.** The "sanitized input becomes unsafe again" class of bug is a *joint* property of jQuery and the sanitizer. Shared test suites with DOMPurify would catch many of these pre-release.
4. **Revisit on every major browser parser change.** When browsers change how they parse edge-case HTML (e.g., template element semantics, declarative shadow DOM, new foreign-content rules), the equivalence of jQuery's output to its input should be re-verified.
5. **Document security-relevant API surface.** A single page enumerating every jQuery method that ends up calling `innerHTML`-equivalent machinery would help callers reason about their own threat models.
6. **Iterate.** This document should be revisited at every major version bump and after every CVE.

---

## Appendix: Notable historical findings referenced above

| CVE | Year | Summary | Fix version |
| --- | --- | --- | --- |
| CVE-2015-9251 | 2015 | Cross-origin Ajax without explicit `dataType` auto-executed `text/javascript` responses. | 3.0.0 |
| CVE-2019-11358 | 2019 | `jQuery.extend(true, {}, source)` polluted `Object.prototype` via `__proto__`. | 3.4.0 |
| CVE-2020-11022 | 2020 | `htmlPrefilter` regex rewrote self-closing tags in ways that could make sanitized HTML execute script. | 3.5.0 |
| CVE-2020-11023 | 2020 | Same class as 11022, specifically triggered by HTML containing `<option>` elements. Later added to CISA's KEV catalog. | 3.5.0 |
