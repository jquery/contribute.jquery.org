<script>{
	"title": "Contributing to jQuery Foundation Documentation"
}</script>

For almost as long as jQuery has been around, its documentation has been an integral part of the project, from helping new users get up to speed to offering detailed explanations of features for seasoned developers. Currently, the jQuery Foundation manages API documentation and demos for its projects and provides a growing set of tutorials on its learning site.

## Getting Involved

If you'd like to help us improve the documentation of any of jQuery's projects, we would love to have your contributions. All of our documentation sites are on [GitHub](https://github.com/jquery), and their repository names match their website domains. For example, you can find jQuery UI documentation at [jqueryui.com](http://jqueryui.com) and [api.jqueryui.com](http://api.jqueryui.com), so their repositories are located at [github.com/jquery/jqueryui.com](https://github.com/jquery/jqueryui.com) and [github.com/jquery/api.jqueryui.com](https://github.com/jquery/api.jqueryui.com). Before you do anything else, you ought to [sign up for a free GitHub account](https://github.com/signup/free) if you don't have one already. If you're comfortable with git and GitHub, you can dive right in by forking a repo, making some changes, and sending a pull request. If not, here are some suggestions for getting involved:

### Report an Issue

Perhaps the easiest way to help is to report an issue or problem about the current documentation. We use GitHub's issue tracker for all of our documentation sites. Just head on over to one of the following issue lists to see if your issue has already been reported, and if it hasn't, give us a detailed rundown of the issue:

* jquery.com — [View Issues](https://github.com/jquery/jquery.com/issues) | [Report Issue](https://github.com/jquery/jquery.com/issues/new)
* api.jquery.com — [View Issues](https://github.com/jquery/api.jquery.com/issues) | [Report Issue](https://github.com/jquery/api.jquery.com/issues/new)
* jqueryui.com — [View Issues](https://github.com/jquery/jqueryui.com/issues) | [Report Issue](https://github.com/jquery/jqueryui.com/issues/new)
* api.jqueryui.com — [View Issues](https://github.com/jquery/api.jqueryui.com/issues) | [Report Issue](https://github.com/jquery/api.jqueryui.com/issues/new)
* jquerymobile.com — [View Issues](https://github.com/jquery/jquerymobile.com/issues) | [Report Issue](https://github.com/jquery/jquerymobile.com/issues/new)
* api.jquerymobile.com — [View Issues](https://github.com/jquery/api.jquerymobile.com/issues) | [Report Issue](https://github.com/jquery/api.jquerymobile.com/issues/new)
* qunitjs.com — [View Issues](https://github.com/jquery/qunitjs.com/issues) | [Report Issue](https://github.com/jquery/qunitjs.com/issues/new)
* api.qunitjs.com — [View Issues](https://github.com/jquery/api.qunitjs.com/issues) | [Report Issue](https://github.com/jquery/api.qunitjs.com/issues/new)
* learn.jquery.com — [View Issues](https://github.com/jquery/learn.jquery.com/issues) | [Report Issue](https://github.com/jquery/learn.jquery.com/issues/new)

### Write or Edit Content

If you actually want to make the fixes and improvements, then
you'll need to [fork](https://help.github.com/articles/fork-a-repo) the
repositories you'd like to work on. Once you've made changes you'd like
reviewed for integration, submit a [pull
request](https://help.github.com/send-pull-requests/). However, we recommend
that you file issues for new "features" and significant changes before actually
getting to work. For more information on maintaining your fork and strategies on
committing, see the [Commits and Pull Requests](/commits-and-pull-requests/)
guide.

All of the API sites use XML for their documentation, while most of the others
use a combination of HTML and [Markdown](http://en.wikipedia.org/wiki/Markdown).
Familiarizing yourself with Markdown is a good idea in general, and it would
certainly facilitate your contributions to our sites.

Each of the documentation repositories comes with a build process that uses the
[node.js](http://nodejs.org) task-based build tool [grunt](http://gruntjs.com).
It's a good practice to run the `grunt` command after changing content because
the command runs a "lint" program that checks for invalid XML and other errors.
For small changes, you probably won't need to see the formatted result in a web
page. For more significant changes, however, it's a good idea to do so before
submitting a pull request in order to avoid mistakes and oversights. All of our
sites use WordPress, and we employ the Grunt build system rather than the
WordPress admin area to update it. For more information about setting up
node.js, Grunt, Wordpress, or any other aspect of your local development
environment for working on jQuery sites, see
[Contributing to Web Sites](/web-sites/).


## Getting Help

If you have any questions about getting things set up or if you're not sure about how to address a problem with the documentation, we're here to help.

The best place to get help is on [IRC](http://en.wikipedia.org/wiki/Internet_Relay_Chat) in the #jquery-content
channel on [Freenode](http://freenode.net). If you're unfamiliar with IRC, you can use the [webchat gateway](http://webchat.freenode.net/).

In addition, the jQuery Content Team holds a [public meeting](http://jquery.org/meeting/) every two weeks on Freenode, at 1PM Eastern time in the `#jquery-meeting` channel.

If IRC is not your thing, but you still want to get in touch, please use the site's GitHub repo or send us an e-mail to `content at jquery dot org`.
