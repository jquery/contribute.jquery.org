<script>{
	"title": "Contributing to jQuery Foundation Web Sites"
}</script>

Just like our JavaScript libraries, we maintain the design and content of all our websites in the open, with everything available on GitHub. We do this for several reasons:

* **It works for code.** Open source development is collaborative, auditable,
and decentralized -- all qualities that should be part of working on design and
documentation as well.
* **Wikis and CMSes are fraught with problems for community projects.** Both
are targets for spammers, which leads to tight control over user
accounts, compounding the problem that there is only a small circle of people around
the project maintainers with the ability to work on content and design. Even if
you desperately want to start contributing to a project, it can take a while
before you encounter the right people and gain the trust to get set up with
credentials. If you're one of the people who have credentials, it's Yet Another
Login and Password to remember.  Once logged in, you have to make your changes
in a `textarea` or RTE instead of where you'd **really** like to be working:
your text editor. Then, if you actually have major changes you want to propose,
you can't, because there's **only one instance** running, so it's no place for
experimentation. Designs stagnate and bugs linger (often untracked) while the
canonical store of content is trapped in a database. Better hope you've got
backups in place, and that you don't ever want to work while you don't have an
internet connection.
* **We want more people to learn how to _do_ open source.**  Our users come
from all backgrounds, and many come to jQuery without prior experience in the
world of commits and pull requests, of local development and dependency
management, of build processes and deployments. This can make contributing to
jQuery seem a remote and intimidating prospect: "Not only do I have to be
capable of fixing a bug in jQuery, I have to figure out all this _stuff_ too?"
With open content and design, we hope to provide opportunities for all you
folks who have "always wanted to get involved" learn the tools and workflows
common to open source projects, while working on the types of issues you're
comfortable solving.

---

## How It Works

Here's the short version: _static content is built into HTML and deployed to a
[WordPress](http://wordpress.org) instance over
[XML-RPC](http://en.wikipedia.org/wiki/XML-RPC) using
[grunt](http://gruntjs.com)_.

### Content Repositories

Inspired by static site generators like [Jekyll](http://jekyllrb.com/), each of
the sites (with some [exceptions*](#exceptions)), on `jquery.com` and
`jquery.org`, including [this very one you're
reading](https://github.com/jquery/contribute.jquery.org), has a corresponding
repository in [our GitHub organization](https://github.com/jquery) that serves
as the canonical source of the content. Most of these repositories have a
combination of HTML and [Markdown](http://en.wikipedia.org/wiki/Markdown)
(typically in the `pages` directory), each with bits of leading metadata (formatted as JSON). The API documentation for
[jQuery](https://github.com/jquery/api.jquery.com), [jQuery
UI](https://github.com/jquery/api.jqueryui.com), and [jQuery
Mobile](https://github.com/jquery/api.jquerymobile.com) is maintained as XML in the
`entries` directory. Assets such as images (the kind you'd put in an `img` tag,
not in a CSS `background-image`) are kept in the `resources` directory.

### WordPress and [jquery-wp-content](https://github.com/jquery/jquery-wp-content)

The sites are served using a [WordPress](http://wordpress.org/) install. We've worked together with members of the WordPress Core Team to
create [jquery-wp-content](https://github.com/jquery/jquery-wp-content), which
provides themes and plugins for all domains and subdomains, using
a [parent/child theme](http://codex.wordpress.org/Child_Themes) setup that
also handles aspects of site configuration automatically.

We do not make **any** modifications to content, layout, or
configuration through the WordPress Admin panels.

### [grunt](http://gruntjs.com): Getting Static Content into WordPress

Each of the content repositories comes with a build process that uses the
[node.js](http://nodejs.org) task-based build tool [grunt](http://gruntjs.com).
Running the `grunt deploy` command will use the two grunt plugins we've built
specifically for our sites,
[grunt-jquery-content](https://github.com/jquery/grunt-jquery-content) and
[grunt-wordpress](https://github.com/scottgonzalez/grunt-wordpress), to process
the static content into HTML, and then synch the built output into WordPress
over [XML-RPC](http://codex.wordpress.org/XML-RPC_Support),  using the
credentials in the `config.json` file in the repository, creating and editing
posts and pages mirroring the directory structure of the original source
content.

### Deploying to Staging and Production Environments

In addition to our production environment, we also have a staging environment
for all of the sites. The URLs for the staging sites are the same as the
production URLs, except they are preceded with a `stage.` prefix, e.g.,
[stage.contribute.jquery.org](https://stage.contribute.jquery.org). We use [git
post-receive hooks](https://help.github.com/articles/post-receive-hooks) to
automate deployment to both environments. Whenever there is a commit to the
`main` branch of any content repository, or
[jquery-wp-content](https://github.com/jquery/jquery-wp-content) itself, the
changes are pulled onto the staging site, and the `grunt deploy` runs, making
them available for immediate preview.

Most sites deploy to both staging and production directly from the `main` branch (or e.g. the `x-y` branches for jQuery UI API and jQuery Mobile API versioned content).

Some sites only deploy to production when a commit is [tagged](http://git-scm.com/book/en/Git-Basics-Tagging) in the Git
repo with a valid [semver](https://semver.org/). Refer to [Infrastructure docs](https://github.com/jquery/infrastructure-puppet/blob/staging/doc/wordpress.md#doc-sites) and look for "semver-tags" to identify these. This includes:

* `jquery.com`
* `api.jquery.com`
* `jqueryui.com`

On api.jqueryui.com and api.jquerymobile.com, the major and minor versions
of the semver tag should match the code version that the site documents. Use `patch` freely for any updates to documentation within given major/minor release cycle.

To create a tag, use `npm version [major | minor | patch]`.

Afterwards, make sure to push both version change commit and the tag:
`git push --tags origin main`

---

## How Can I Help?

### Just File Issues!

When you notice something wrong with one of our sites, or you have an idea for
how something could be improved, don't keep it to yourself. Filing a [Github
issue](https://github.com/features/projects/issues) on the appropriate
repository is the best way to let us know what's up.

When you're looking at a live site, it may sometimes be a bit unclear whether
your issue should be filed on a content repository or directly on
[jquery-wp-content](https://github.com/jquery/jquery-wp-content). Typically, if
the problem has anything to do with markup or CSS, it's a `jquery-wp-content`
problem. If the problem lies with the actual content of the words and code
you're reading, you probably should file the issue on the content repository.
If you happen to make the "wrong" choice, however, we still appreciate the report, and
will make sure it finds its way to the right place if necessary.

### Editing and Authoring Content

If you actually want to make commits to make the fixes and improvements, then
you'll need to [fork](https://help.github.com/articles/fork-a-repo) the content
repositories you'd like to work on.  When you have changes you'd like to have
reviewed for integration, submit a [pull
request](https://help.github.com/send-pull-requests/). However, we recommend
that you file issues for new "features" and significant changes before actually
getting to work. For more information on maintaining your fork and strategies on
commiting, see the [Commits and Pull Requests](/commits-and-pull-requests/)
guide.

You'll note that it is possible to make content changes without setting up a
local WordPress instance. You won't be able to preview your changes as they'll
appear on the site, and you won't be able to run `grunt deploy`, which means
you can't be 100% sure that the content will build successfully. We will accept
pull requests on content from users who haven't had a chance to test locally, but
encourage anyone who's planning to contribute regularly to get configured
for deploying and testing locally.

### Design and Layout

To work on the CSS, HTML, and PHP that comprise the site theme, you'll need a
fork of [jquery-wp-content](https://github.com/jquery/jquery-wp-content). Again,
it is possible, but discouraged, to make theming changes without configuring a
local WordPress instance.

The parent theme that applies to all sites is located in the `themes/jquery`
folder. Each content site has a child theme in the `themes` folder that
corresponds to the repository name, and supplies styling and templates specific
to that site.

---

## Local Development

In order to iterate on site content and design in the same way that jQuery team
members do, we encourage you to setup a local WordPress instance using
[jquery-wp-content](https://github.com/jquery/jquery-wp-content) as described in its [README](https://github.com/jquery/jquery-wp-content/#jquery-wp-content).

### Workflow

_These setup instructions apply to all jQuery Foundation websites with public
content repositories. For the sake of the rest of this example, we'll assume
you want to work on the content and style of
[contribute.jquery.org](http://contribute.jquery.org). Please substitute the URL
of whichever site you are actually working on, as appropriate._

Once you get [jquery-wp-content](https://github.com/jquery/jquery-wp-content) working, you should be able navigate to
a site in your browser that looks exactly like the live site, only without any content.

Now we need to populate your local WordPress with the content from the [`contribute.jquery.org`](https://github.com/jquery/contribute.jquery.org) repo.

1. Fork the [`contribute.jquery.org`](https://github.com/jquery/contribute.jquery.org) repository on GitHub by clicking the "Fork" button.
1. Clone your forked repository on your machine wherever you'd like (just *not* inside of your WordPress or `jquery-wp-content` directories).<br> `git clone https://github.com/YourUsername/contribute.jquery.org.git`
1. Enter the directory where you cloned the repo <br>`cd contribute.jquery.org`
1. Install grunt-cli (if you haven't already) <br>`npm install -g grunt-cli`
1. Install local build dependencies <br>`npm install`
1. Copy the `config-sample.json` file to `config.json` in the same directory <br>`cp config-sample.json config.json`
1. Edit `config.json` to use the URL, username, and password for your local WordPress.
1. Build and deploy the files to your local WordPress -- `grunt deploy`

At this point your local WordPress should be populated with the [`contribute.jquery.org`](https://github.com/jquery/contribute.jquery.org) content.

When working on content locally, you may find it useful to use the `grunt watch` task,
which will re-deploy the site every time you edit any of the content files.

Make your changes to your the content repo or to `jquery-wp-content` as
necessary, and keep the [Commits and Pull
Requests](/commits-and-pull-requests/) guidelines in mind as you prepare your
work.

---

## Getting Help

If you're struggling to get any part of any site working properly, or have any questions, we're here to help.

The best place to get help is in the [jQuery Chat](https://jquery.com/support/) on IRC or Matrix. Alternatively, use the specific site's GitHub repo to report an issue.

---

## Site & Repository Guide

* [jquery-wp-content](https://github.com/jquery/jquery-wp-content/) - The WordPress configuration and themes that serve all our documentation sites

For a current list of jQuery doc sites, refer to [Infrastructure docs](https://github.com/jquery/infrastructure-puppet/blob/staging/doc/wordpress.md#doc-sites):

* [jquery.com](https://github.com/jquery/jquery.com/) - The content of [jquery.com](http://jquery.com) itself
* [api.jquery.com](https://github.com/jquery/api.jquery.com/) - jQuery API Documentation
* [learn.jquery.com](https://github.com/jquery/learn.jquery.com/) - jQuery Learning Center
* [releases.jquery.com](https://github.com/jquery/releases.jquery.com/) - jQuery CDN file browser

* [jqueryui.com](https://github.com/jquery/jqueryui.com/) - The content of [jqueryui.com](http://jqueryui.com) itself
* [api.jqueryui.com](https://github.com/jquery/api.jqueryui.com/) - jQuery UI API Documentation

* [jquerymobile.com](https://github.com/jquery/jquerymobile.com/) - The content of [jquerymobile.com](http://jquerymobile.com) itself
* [api.jquerymobile.com](https://github.com/jquery/api.jquerymobile.com/) - jQuery Mobile API Documentation

* [jquery.org](https://github.com/jquery/jquery.org/) - The content of [jquery.org](http://jquery.org) itself
* [brand.jquery.org](https://github.com/jquery/brand.jquery.org/)
* [contribute.jquery.org](https://github.com/jquery/contribute.jquery.org/) - The content of the [jQuery Contribution Hub](http://contribute.jquery.org) site
* [meetings.jquery.org](https://github.com/jquery/meetings.jquery.org/)

<a name="exceptions"></a>
### Other sites

The following sites are not managed by the system described in this article. They are content sites that used the system in the past and moved to a different system, werre archived to a static site, or are standalone applications.

* [bugs.jquery.com](http://bugs.jquery.com) - Standalone
* [plugins.jquery.com](https://github.com/jquery/plugins.jquery.com/) - Archived
* [bugs.jqueryui.com](http://bugs.jqueryui.com) - Standalone
* [download.jqueryui.com](https://github.com/jquery/download.jqueryui.com/) - Standalone
* [events.jquery.org](https://github.com/jquery/events.jquery.org/) - Archived
* [irc.jquery.org ](https://github.com/jquery/irc.jquery.org/tree/v1.2.2) - Archived
* [qunitjs.com](https://github.com/qunitjs/qunitjs.com/tree/v2.10.2) - Moved to a different system
* [api.qunitjs.com](https://github.com/qunitjs/api.qunitjs.com/tree/v2.3.0) - Moved to a different system
