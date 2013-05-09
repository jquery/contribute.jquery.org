---
title: Contributing to jQuery Foundation Web Sites
---

Just like our JavaScript libraries, we maintain the design and content of all our websites in the open, with everything available on GitHub. We do this for several reasons:

* **It works for code.** Open source development is collaborative, auditable,
and decentralized -- all qualities that should be part of working on design and
documentation as well.
* **Wikis and CMSes are fraught with problems for community projects.** Both
are targets for spammers, which leads to tight control over user
accounts, compounding the problem that and only a small circle of people around
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

## <a name="how-it-works">How It Works</a>

Here's the short version: _static content is built into HTML and deployed to a
[WordPress](http://wordpress.org) instance over
[XML-RPC](http://en.wikipedia.org/wiki/XML-RPC) using
[grunt](http://gruntjs.com)_.

### Content Repositories

Inspired by static site generators like [Jekyll](http://jekyllrb.com/), each of
the sites (with some exceptions[*](#exceptions)), on `jquery.com` and
`jquery.org`, including [this very one you're
reading](https://github.com/jquery/contribute.jquery.org), has a corresponding
repository in [our GitHub organization](http://github.com/jquery) that serves
as the canonical source of the content. Most of these repositories have a
combination of HTML and [Markdown](http://en.wikipedia.org/wiki/Markdown)
(typically in the `pages` directory), each with bits of leading metadata (JSON
or [YAML](http://www.yaml.org/), respectively).  The API documentation for
[jQuery](http://github.com/jquery/api.jquery.com), [jQuery
UI](http://github.com/jquery/api.jqueryui.com), [jQuery
Mobile](http://github.com/jquery/api.jquerymobile.com), and
[QUnit](http://github.com/jquery/api.qunitjs.com) is maintained as XML in the
`entries` directory. Assets such as images (the kind you'd put in an `img` tag,
not in a CSS `background-image`) are kept in the `resources` directory.

### WordPress and [jquery-wp-content](http://github.com/jquery/jquery-wp-content)

Whether for production, staging, or local development, the sites are served
from a [WordPress network](http://codex.wordpress.org/Glossary#Network). Using
a WordPress multisite setup gives us the control we need to share structure and
style across all of the different sites, and allows us to provides us with
dynamic features like searching and user accounts on top of the static content
repositories. We've worked together with members of the WordPress Core Team to
create [jquery-wp-content](http://github.com/jquery/jquery-wp-content), which
provides a custom install script that sets up all the different subdomains, and
a [parent/child theme](http://codex.wordpress.org/Child_Themes) setup that
applies across the entire network, and handles other aspects of the
configuration.

We do not make **any** modifications to content, layout, or
configuration through the WordPress Administration panels.

### [grunt](http://gruntjs.com): Getting Static Content into WordPress

Each of the content repositories comes with a build process that uses the
[node.js](http://nodejs.org) task-based build tool [grunt](http://gruntjs.com).
Running the `grunt deploy` command will use the two grunt plugins we've built
specifically for our sites,
[grunt-jquery-content](http://github.com/jquery/grunt-jquery-content) and
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
[stage.contribute.jquery.org](http://stage.contribute.jquery.org). We use [git
post-receive hooks](https://help.github.com/articles/post-receive-hooks) to
automate deployment to both environments. Whenever there is a commit to the
`master` branch of any content repository, or
[jquery-wp-content](http://github.com/jquery/jquery-wp-content) itself, the
changes are pulled onto the staging site, and the `grunt deploy` runs, making
them available for immediate preview. When we are ready to deploy changes to
production, we need only [tag](http://git-scm.com/book/en/Git-Basics-Tagging) the
repo with a valid [semver](http://semver.org/), and the same process takes
place in production.

 ---

 ## <a name="how-can-i-help" href="#how-can-i-help">How Can I Help?</a>

 ### Just File Issues!

 When you notice something wrong with one of our sites, or you have an idea for
 how something could be improved, don't keep it to yourself. Filing a [Github
 issue](https://github.com/features/projects/issues) on the appropriate
 repository is the best way to let us know what's up. We even have repositories
 just for tracking issues with sites that aren't managed through
 [jquery-wp-content](http://github.com/jquery/jquery-wp-content), such as
 [http://bugs.jquery.com](http://github.com/jquery/bugs.jquery.com).

 When you're looking at a live site, it may sometimes be a bit unclear whether
 your issue should be filed on a content repository or directly on
 [jquery-wp-content](http://github.com/jquery/jquery-wp-content). Typically, if
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
request](http://help.github.com/send-pull-requests/). However, we recommend
that you file issues for new "features" and significant changes before actually
getting to work. For more information on maintaining your fork and strategies on
commiting, see the [Commits and Pull Requests](/commits-and-pull-requests/)
guide.

You'll note that it is possible to make content changes without setting up a
local WordPress instance. You won't be able to preview your changes as they'll
appear on the site, and you won't be able to run `grunt deploy`, which means
you can't be 100% sure that the content will build successfuly. We will accept
pull requests on content from users who haven't had a chance to test locally, but
encourage anyone who's planning to contribute regularly to get configured
for deploying and testing locally.

### Design and Layout

To work on the CSS, HTML, and PHP that comprise the site theme, you'll need a
fork of [jquery-wp-content](http://github.com/jquery/jquery-wp-content). Again,
it is possible, but discouraged, to make theming changes without configuring a
local WordPress instance.

The parent theme that applies to all sites is located in the `themes/jquery`
folder. Each content site has a child theme in the `themes` folder that
corresponds to the repository name, and supplies styling and templates specific
to that site.

---

## <a name="local-development" href="#local-development">Local Development</a>

In order to iterate on site content and design in the same way that jQuery team
members do, we encourage anyone to set up a local WordPress instance running
[jquery-wp-content](http://github.com/jquery/jquery-wp-content).

Before you get started, you'll need to have the following technologies installed on your
system

* [node.js](http://nodejs.org) >= 0.8.x
* Local PHP/MySQL environment, e.g., [MAMP](http://www.mamp.info/en/index.html), [XAMPP](http://www.apachefriends.org/en/xampp.html)

Once you've got these major pieces are in place, you'll be able to get
WordPress and [jquery-wp-content](http://github.com/jquery/jquery-wp-content)
following [the instructions in its
README](https://github.com/jquery/web-base-template/blob/master/README.md).

### <a name="workflow" href="#workflow">Workflow</a>

_These setup instructions apply to all jQuery Foundation websites with public
content repositories. For the sake of the rest of this example, we'll assume
you wanted to work on the content and style of
[contribute.jquery.org](http://contribute.jquery.org). Please substitute the URL
of whichever site you are actually working on, as appropriate._

Once you get [jquery-wp-content](http://github.com/jquery/jquery-wp-content) working right, you should be able navigate to
[local.contribute.jquery.org](http://local.contribute.jquery.org) in a browser,
and see a site that looks exactly like the live site, only without any actual articles. That's where
your fork `contribute.jquery.org` content repo comes into play.

1. Fork the [`contribute.jquery.org`](https://github.com/jquery/contribute.jquery.org) repository on GitHub by clicking the "Fork" button.
1. Clone your forked repository to wherever you'd like, except *not* be inside of your WordPress and `jquery-wp-content` directories. -- `git clone https://github.com/YourUsername/contribute.jquery.org.git`
1. Enter the directory where you cloned the repo -- `cd contribute.jquery.org`
1. Install grunt-cli (if you haven't already) -- `npm install -g grunt-cli`
1. Install local build dependencies -- `npm install`
1. Copy the `config-sample.json` file to `config.json` in the same directory -- `cp config-sample.json config.json`
1. Edit `config.json` to use the username and password for your local WordPress network
1. Build and deploy the files to your local WordPress -- `grunt deploy`

At this point, if you refresh your `local.contribute.jquery.org`, your local
instance should be populated with all of the site content.

When working on content locally, you may find it useful to use the `grunt watch` task,
which will re-deploy the site every time you edit any of the content files.

Make your changes to your the content repo or to `jquery-wp-content` as
necessary, and keep the [Commits and Pull
Requests](/commits-and-pull-requests/) guidelines in mind as you prepare your
work.

---

## Getting Help

If you're struggling to get any part of any site working properly, or have any questions, we're here to help.

The best place to get help is on [IRC](http://irc.jquery.org/), in the
[#jquery-content](irc://irc.freenode.net/#jquery-content) channel on
[Freenode](http://freenode.net). If you're unfamiliar with IRC, you can use the
[webchat gateway](http://webchat.freenode.net/) or [learn
more](http://irc.jquery.org/irc-help/).

In addition, the jQuery Content Team holds a [public, weekly
meeting](http://jquery.org/meeting) on Freenode, at 1PM Eastern time in the #jquery-meeting channel.

If IRC is not your thing, but you still want or need to get in touch, please use the site's GitHub repo or send us an e-mail to `content at jquery dot org`.

---

## <a name="site-repository-guide" href="#site-repository-guide">Site &amp; Repository Guide</a>

* [jquery-wp-content](http://github.com/jquery/jquery-wp-content/) - The WordPress configuration and themes that serve all our sites


* [jquery.com](http://github.com/jquery/jquery.com/) - The content of [jquery.com](http://jquery.com) itself
* [api.jquery.com](http://github.com/jquery/api.jquery.com/) - jQuery Core API documentation
* [plugins.jquery.com](http://github.com/jquery/jquery.com/) - The jQuery Plugin Registry
* [learn.jquery.com](http://github.com/jquery/learn.jquery.com/) - The jQuery Learning Center


* [jqueryui.com](http://github.com/jquery/jqueryui.com/) - The content of [jqueryui.com](http://jqueryui.com) itself
* [api.jqueryui.com](http://github.com/jquery/api.jqueryui.com/) - jQuery UI API documentation
* [download.jqueryui.com](http://github.com/jquery/download.jqueryui.com/) - The [jQuery UI Download Builder](http://download.jqueryui.com)


* [jquerymobile.com](http://github.com/jquery/jquerymobile.com/) - The content of [jquerymobile.com](http://jquerymobile.com) itself
* [api.jquerymobile.com](http://github.com/jquery/api.jquerymobile.com/) - jQuery Mobile API documentation


* [qunitjs.com](http://github.com/jquery/qunitjs.com/) - The content of [qunitjs.com](http://qunitjs.com) itself
* [api.qunitjs.com](http://github.com/jquery/api.qunitjs.com/) - QUnit API documentation


* [sizzlejs.com](http://github.com/jquery/sizzlejs.com/) - The content of [sizzlejs.com](http://sizzlejs.com) itself


* [jquery.org](http://github.com/jquery/jquery.org/) - The content of [jquery.org](http://jquery.org) itself
* [contribute.jquery.org ](http://github.com/jquery/contribute.jquery.org/) - The content of the [jQuery Contribution Hub](http://contribute.jquery.org) site
* [events.jquery.org](http://github.com/jquery/jquery.org/) - The content of our [Events and Conferences](http://events.jquery.org) site
* [irc.jquery.org ](http://github.com/jquery/irc.jquery.org/) - The content of our [IRC log and information](http://irc.jquery.org) site

### Non-conforming Sites

Some of our sites are not part of the system described in this article. Some
because they are simply not yet live, others still have some transitional work
still in progress, and others that can't be integrated or have been deprecated.

#### Not Live, Content Migration In Progress/Planned

* [blog.jquery.com](http://blog.jquery.com)
* [blog.jqueryui.com](http://blog.jqueryui.com)
* [jquerymobile.com](http://jquerymobile.com)
* [blog.jquerymobile.com](http://blog.jquerymobile.com)
* [sizzlejs.com](http://sizzlejs.com)

#### Will Not Be Transitioned To Content Repository, Still Needs New Theming

* [forum.jquery.com](http://forum.jquery.com)
* [bugs.jquery.com](http://bugs.jquery.com)
* [bugs.jqueryui.com](http://bugs.jqueryui.com)

#### Deprecated, Shutdown Imminent

* [docs.jquery.com](http://docs.jquery.com)
* [meetups.jquery.com](http://meetups.jquery.com)

