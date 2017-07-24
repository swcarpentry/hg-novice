---
title: Automated Version Control
teaching: 10
exercises: 5
questions:
- "What is version control and why should I use it?"
objectives:
- "Understand the benefits of an automated version control system."
- "Understand the basics of how [Mercurial](https://www.mercurial-scm.org/) works."
keypoints:
- "Version control is like an unlimited 'undo'."
- "Version control also allows many people to work in parallel."
---

We'll start by exploring how version control can be used
to keep track of what one person did and when.
Even if you aren't collaborating with other people,
automated version control is much better than this situation:

[![Piled Higher and Deeper by Jorge Cham, http://www.phdcomics.com](../fig/phd101212s.gif)](http://www.phdcomics.com)

"Piled Higher and Deeper" by Jorge Cham, http://www.phdcomics.com

We've all been in this situation before:
it seems ridiculous to have multiple nearly-identical versions of the same
document.
Some word processors let us deal with this a little better,
such as Microsoft Word's "Track Changes" or Google Docs' version history.

Version control systems start with a base version of the document and then save
just the changes you made at each step of the way.
You can think of it as a tape:
if you rewind the tape and start at the base document,
then you can play back each change and end up with your latest version.

![Changes Are Saved Sequentially](../fig/play-changes.svg)

Once you think of changes as separate from the document itself,
you can then think about "playing back" different sets of changes onto the base
document and getting different versions of the document.
For example,
two users can make independent sets of changes based on the same document.

![Different Versions Can be Saved](../fig/versions.svg)

If there are no conflicts, you can even try to play two sets of changes onto
the same base document.

![Multiple Versions Can be Merged](../fig/merge.svg)

A version control system is a tool that keeps track of these changes for us and
helps us version and merge our files. It allows you to
decide which changes make up the next version, called a
[commit]({{ page.root }}/reference/#commit), and keeps useful metadata about them. The
complete history of commits for a particular project and their metadata make up
a [repository]({{ page.root }}/reference/#repository). Repositories can be kept in sync
across different computers facilitating collaboration among different people.

> ## The Long History of Version Control Systems
>
> Automated version control systems are nothing new. Tools like RCS, CVS, or
> Subversion are considered now legacy systems, offering more limited
> capabilities than modern tools, such as
> [Mercurial](https://www.mercurial-scm.org/) and
> [Git](http://swcarpentry.github.io/git-novice/).
> In particular,
> the latter are *distributed*, meaning that they don't need a centralized
> server to host the repository.
{: .callout}
