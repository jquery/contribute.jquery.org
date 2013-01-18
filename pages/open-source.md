---
title: Getting Started Contributing to Open Source
---

Contributing to the jQuery project, and to any FOSS project for that matter, can be a rewarding experience both in terms of the way you can help yourself and projects you are working on, as well as the countless number of others you may help with your contribution. Taking that first step though, can be intimidating. From learning new technologies, to interacting with people you have never met, to putting yourself and your code out for the world to see, there are many factors that may act as barriers to entry. Hopefully, this document will give you that push you need to get past those reservations and get involved.

## Dive In

Usually, someone has an idea of what they want to contribute to an open source project before they even think about how they would do it. But even if you have no idea what you want to contribute, there is probably some way that you can make your mark.

The obvious way is to contribute to the code base. Perhaps you have been fixing bugs in a particular piece of software that you use in your projects. Maybe you have created a new addition to a library that you think will be helpful to others as part of the larger project. These and many other types of every day tasks that programmers do are perfect examples of ways to give back to the open source community.

But you don't necessarily have to write code to contribute. Projects are almost always looking for help with creating or updating documentation and the web sites and other items that accompany them. If someone doesn't know why a project exists or how to use it, what reason would they have to give it a try? Telling someone in an effective way what a project is for and how to use it is just as important as providing the code for them to use. And it doesn't end there. From helping to maintain infrastructure to event planning, projects need help in many areas beyond the code.

## Join the Conversation

So now you know what you want to contribute but you're not sure where to start. Most projects provide a number of ways for you to discuss features, express concerns or just chat about the project with other people just like you.

### Reporting and Fixing Bugs

At least you think you found a bug. The best thing to do is to first try to narrow that issue down to as minimal of a test case as you can to make sure it's an issue with the project. You wouldn't want someone to tell you that you did something wrong if they didn't have proof that it's your fault, right? Once you know it's the projects issue, make sure they don't already know about it by checking their system for bug tracking. There may already be a discussion in progress that you can contribute to or at least follow for a resolution. If the project doesn't know about this bug, then it's time to tell them about it. Projects usually have a preferred method for reporting bugs so try to follow that method and let them know about it. And better yet, provide the fix too!

### Contributing Something New

So you have added a feature to a library or you have created something new based on someone elses work or maybe you just have some bit of code that you use all the time in your projects and now you want to give that to the community and let them enjoy the same benefits you have. Great! This is how open source projects begin, grow, thrive and reach new users.

If you've created something new, put it out there! Shout it from the mountain tops and get people to use and contribute to it. If you're wanting to contribute something to an existing project though, let's slow it down a bit. There are a few questions you might need to ask yourself, or the maintainer(s) of the project may ask you.

**How does this change help others?** Most likely the answer to this question is the same way the change helped you. Keep in mind though that not everyone is in the same situation as you and if your solution is too specific to your needs it may not be seen as something beneficial to everyone else.

**Why would the project maintain this code for you?** Let's face it, any addition to a library is more work for the people maintaining that project. So what benefit does the project get from your contribution? It could be more users or the opportunity to catch up to a competing project or the ability to fill a gap in the project's functionality. What ever the reason, it should be a compelling one to convince the project team that they should take that code off of your hands and merge it with theirs.

And what if the project decides not to include your contribution? That's ok! You can still use it yourself. You can still, within guidelines of any license that may be in place, distribute it yourself. There are ways to contribute to a project and its community without actually having your bits merged.

### Being a Good Citizen

When you contribute to open source projects, you become part of a community. A group of like-minded individuals coming together over a common interest or skill set. Just like in any community, there are appropriate ways to conduct yourself when interacting with other members of that group. Treating others with respect is not just curtious, but it helps encourage new people that may have important contributions to feel comfortable enough to put themselves out there and take that first step toward getting involved.

Whether you're answering questions in a forum, chatting with someone at a conference or commenting on a section of someone's code, keep in mind that your words and your actions have real consequences. Treat others how you would like to be treated and keep in mind that we were all beginners at some point. When you encounter that person that doesn't follow these recommendations, and don't worry, you will, just try to follow these same recommendations. Who knows, maybe that person has the solution you've been looking for or has a great idea for something new and they just need to be heard.

## Add to Your Toolbelt

Just like any other skilled laborer, an open source contributor has a few tools of the trade that they need to learn. As with other facets of contributing to open source, learning and using some of these tools may be intimidating at first but remember, you're part of a community now. More than likely, there is someone out there that had the same problems as you and is eagerly awaiting the chance to share with you how they overcame those problems and how you can too.

### The Command Line

When learning many of these tools, at some point it becomes inevitable that you will need to become more familiar and comfortable with the command line. There is no doubt that many of the tools we use have very good GUI applications and when someone is starting out in open source, the initial thought may be to go that route. Eventually though, the need to become comfortable with the command line grows. Whether you're managing your files, editing their contents or interacting with code repositories, at some point the more granular control and efficient interface of the command line becomes a warm blanket you wrap yourself in when you sit down to work. So why not start with the command line and avoid the process of relearning everything when you decide that you need the command line? You'll be glad did.

### Local Development

Many open source projects use [git](http://git-scm.com/) for managing their source code. Working locally on that code, instead of on the actual "origin" source files may take some getting used to but is not a major hurdle to leap over. When submitting changes to the originating repository, a developer does what is called a Pull Request which, as the name describes, asks the owners of the repo to "pull" the changes from your copy of the files into theirs.

Within git, one of your best friends will be the branch. A branch is a sort of snapshot in time of the code you are working on. The main branch is called the master branch and you can create any number of addition branches. When preparing to make changes for a pull request it is highly recommended to make those changes in a branch other than master. That way, your master branch isn't stuck with your changes awaiting approval. This can lead to all kinds of messes like not being able to work on other items or creating branches off of the modified master branch and thus including possibly unapproved changes in later pull requests. Git is a powerful tool and we could write an entire article on it ... oh wait, [we did](../commits-and-pull-requests).
