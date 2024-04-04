<script>{
	"title": "How to Report Bugs"
}</script>

## Before You Report

### Make Sure It's a jQuery Bug

Many bugs reported to our bug trackers are actually bugs in user code, not in jQuery code. Keep in mind that just because your code throws an error and the console points to a line number inside of a jQuery project, this does not mean the bug is a jQuery bug; more often than not, these errors result from providing incorrect arguments when calling a jQuery function.

If you are new to jQuery, you may want to first ask for help in the [jQuery Chatroom](https://jquery.com/support/) on IRC or Matrix. This might solve your problem without needing to report a bug, or otherwise may help you confirm that is indeed a bug in jQuery, before reporting it as an issue.

### Disable Any Browser Extensions

Make sure you have reproduced the bug with all browser extensions disabled, as these can sometimes cause things to break in interesting and unpredictable ways.

In Firefox, restart in [safe mode](http://kb.mozillazine.org/Safe_mode). In Internet Explorer, follow [these instructions](https://support.microsoft.com/en-us/kb/298931). In Chrome, enter `chrome://extensions` in your address bar and disable all extensions. In Safari, go to Safari -> Preferences -> Extensions and move the slider to "Off".

### Try a Newer Version

Bugs in old versions of jQuery may have already been fixed in newer versions. Make sure to reproduce the bug with the latest version before creating a bug report. You are also welcome to try the latest unreleased git build from <https://releases.jquery.com/jquery/>.

### Try an Older Version

Sometimes, bugs are introduced in newer versions of jQuery that did not exist in previous versions. When possible, it can be useful to try testing with earlier versions in order to isolate when the bug was introduced. Knowing when a behaviour does not happen, can be just as useful as knowing when it does happen.

### Reduce, Reduce, Reduce!

When you are experiencing a problem, the most useful thing you can possibly do is to [reduce your code](http://webkit.org/quality/reduction.html) to the bare minimum required to reproduce the issue. This makes it much easier to isolate and fix the offending code. Bugs that are reported without reduced test cases take on average much longer to fix than bugs that are submitted with them, so you really should try to do this if at all possible.

## When You Report

### Search First

Chances are high that whatever bug you are experiencing has already been reported at least once, so try doing a couple of searches before creating a new bug report.

If the bug has already been reported, check to see if you can provide any new information that the original submitter did not, such as a reduced testcase, link to a demo, a patch, or details about other circumstances under which the bug occurs.

### What to Provide

At minimum, the following information should be provided in any new bug report:

* The version(s) of the jQuery project that are affected.
* The browser (or browsers) that you are able to reproduce the bug in, including version numbers.
* The operating system (or operating systems) you experienced the bug on.
* Step-by-step instructions on how to reproduce the issue, including any required system configuration changes.
* A description of what you expect to happen, and what actually happens.

Ideally, you would also include the following:

* A link to a [reduced](http://webkit.org/quality/reduction.html), working demo/test case that stay online. We recommend [CodePen](https://codepen.io/), [JS Bin](https://jsbin.com/), and [jsFiddle](http://jsfiddle.net/).
* Information on other versions tested, including whether or not the issue is reproducible there.
* Information on other browsers where the issue does not occur.
* A patch (preferably as a link to a [GitHub commit or pull request](/commits-and-pull-requests/)).
* A link to any relevant discussions, such as forum posts.

### One Bug, One Report

Please do not submit multiple bugs in one bug report. When you do this, they need to be separated by a team member, and that takes away from time that could be spent actually fixing the bug.

## Where to Report

* jQuery Core: https://github.com/jquery/jquery/issues
* jQuery Migrate: https://github.com/jquery/jquery-migrate/issues
* jQuery UI: https://github.com/jquery/jquery-ui/issues
* jQuery Mobile: https://github.com/jquery/jquery-mobile/issues
* jQuery Color: https://github.com/jquery/jquery-color/issues
* Sizzle: https://github.com/jquery/sizzle/issues
* QUnit: https://github.com/jquery/qunit/issues
* Globalize: https://github.com/jquery/globalize/issues

All jQuery web sites have a repository named the same as the domain name. For example, the repository for jquery.com is located at https://github.com/jquery/jquery.com. For a list of web site issue trackers, see the [bug triage page](/triage/#web-sites).
