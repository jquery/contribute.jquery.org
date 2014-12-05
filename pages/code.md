<script>{
	"title": "Writing Code for jQuery Foundation Projects"
}</script>

Like many Open Source projects, the majority of jQuery contributors are volunteers; their ability to contribute ebbs and flows with the demands of their professional and personal lives. As a result, jQuery projects are always looking for talented people who are motivated to contribute.

New contributors sometimes make the mistake of starting with a pull request with code that implements some feature they want to be included. In most cases there is a process for features that starts with discussion and design before coding, so the chances are slim that an out-of-the-blue contribution will be accepted. That can leave the contributor feeling that their effort isn't appreciated. We don't want to leave that impression!

By following the steps below, you'll have a better chance at getting your contribution accepted.

**Establish the need for a fix or a feature.** Each project has its own bug/issue tracker where the team keeps its lists of things that need to be done. If there is already an open issue, post on it volunteering to do the coding. If there isn't an issue, create one. For more information, see [How to Report Bugs](http://contribute.jquery.org/bug-reports/).

**Discuss the ticket with the team.** Team members may have advice on the best way to tackle the problem. Once you have agreement on a general plan of attack, have the team member assign the ticket to you. This discussion can happen in the appropriate issue tracker or in [IRC](http://irc.jquery.org/).

**Follow the style guide.** The jQuery [style guides](/style-guide/) describe the style we use for writing all jQuery project code.

**Always add unit tests.** It's rare that new or changed code doesn't need at least one additional unit test. If the code is fixing a bug, there should be a unit test to ensure the problem doesn't recur and that the code actually fixes the problem! If the code is implementing a new feature, it often requires several unit tests that exercise all the code paths.

**Sign the Contributor License Agreement (CLA).** All jQuery Foundation contributors must [sign the CLA](/CLA/) so that the Foundation has a clear legal right to use the code and users of jQuery and projects are clear on the licensing. If you are an employee of a company, you should ensure that the company allows contributions to Open Source projects.

**Create a pull request.** A lot of people make the mistake of working on their master branch. It's much cleaner to create a separate branch for each different feature. Git is something you can spend your whole life learning, but [this tutorial](/commits-and-pull-requests/) will give you a crash course in the essentials.

**Respond to code reviews.** Very few pull requests are landed without discussion. Sometimes a team member may ask for an explanation of the code, which may lead to further requests, for example adding comments that explain the code. Or, a review may identify further optimizations that can be done. You can push new commits to the existing branch and they will automatically update the pull request.

**Bask in the glory!** When your pull request is accepted, point your friends to it and brag. If they're good at code too, tell them to contribute!
