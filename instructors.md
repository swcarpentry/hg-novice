---
layout: page
title: Version Control with Mercurial
subtitle: Instructor's Guide
---
## Legend

Wolfman and Dracula have been hired by Universal Missions
(a space services spin-off of Euphoric State University)
to figure out where the company should send its next planetary lander.
They want to be able to work on the plans at the same time,
but they have run into problems doing this in the past.
If they take turns,
each one will spend a lot of time waiting for the other to finish,
but if they work on their own copies and email changes back and forth
things will be lost, overwritten, or duplicated.

The solution is to use [version control](reference.html#version-control)
to manage their work.
Version control is better than mailing files back and forth because:

*   Nothing that is committed to version control is ever lost.
    This means it can be used like the "undo" feature in an editor,
    and since all old versions of files are saved
    it's always possible to go back in time to see
    exactly who wrote what on a particular day,
    or what version of a program was used to generate a particular set of results.
*   It keeps a record of who made what changes when,
    so that if people have questions later on
    they know who to ask.
*   It's hard (but not impossible) to accidentally overlook or overwrite someone's changes:
    the version control system automatically notifies users
    whenever there's a conflict between one person's work and another's.

This lesson shows how to use
a popular open source version control system called [Mercurial](http://mercurial.selenic.com/)
(also known as `hg`).
It is widely used,
both because it's easy to set up
and because of a hosting site called [Bitbucket](http://bitbucket.org/).
No matter which version control system you use,
the most important thing to learn is not the details of their more obscure commands,
but the workflow that they encourage.


## Overall

*   Time estimate @douglatornell: 2 hours 45 minutes

Version control may be the most important topic that we teach.
We're teaching Mercurial because it offers a consistent user interface,
and the basic workflow requires only a few easy to remember commands.

There are lots of advanced features in Mercurial and other distributed version control systems such as branching,
hashes,
rebasing,
etc.
that we don't teach.
The goal is to introduce the basics so that the learners leave the workshop able to create and use repositories for themselves with confidence,
and that they are aware that version control can also be used to collaborate with other people.

In other words,
try to convince the learners that version control is useful for researchers
working alone or in teams because it is

- a better way to "undo" changes,
- a better way to collaborate than mailing files back and forth, and
- a better way to share your code and other scientific work with the world.

## Teaching Notes

*   Make sure the network is working *before* starting the [Collaborating](02-collab.html) section of this lesson.
    [A Better Kind of Backup](01-backup.html) focuses on individual use of Mercurial on and can be done without network access.
    [Conflicts](03-conflict.html) can also be taught without network access and Bitbucket by creating 2 clones of the repository and pulling changes between them.

*   Drawings are particularly useful in this lesson:
    if you have a whiteboard,
    use it!

*   Version control is usually not the first subject in a workshop,
    so get learners to create a Bitbucket account after the session before.

*   [TortoiseHg](http://tortoisehg.bitbucket.org/) is the recommended means of installing Mercurial for Windows learners.
    The workshop installation instructions should include that.

*   We don't use a Mercurial GUI in these notes because we want learners to understand what commands are being run.
    That said,
    instructors may want to demo the [TortoiseHg](http://tortoisehg.bitbucket.org/) GUI
    (which is available for Windows, OS/X, and Linux)
    on their desktop at some point during this lesson.

*   The [Conflicts](03-conflict.html) section of the lesson uses the [KDiff3](http://kdiff3.sourceforge.net/) graphical diff/merge tool.
    The workshop installation instructions should include directions for OS/X and Linux users to install KDiff3.
    For Windows users it is bundled with TortoiseHg.

*   When appropriate, explain that we teach Mercurial rather than CVS,
    Subversion,
    or Git primarily because of its consistent command-line interface,
    easy to learn basic workflow,
    and Bitbucket's policy of providing unlimited collaboration in private repositories for academic users.
    CVS and Subversion are now seen as legacy systems.
    Git is also very popular,
    largely because of the GitHub service.
    The differences between Mercurial and Git are largely syntactic,
    so learning one provides the conceptual framework to use either.


## [A Better Kind of Backup](01-backup.html)

*   Ask, "Who uses 'undo' in their editor?"
    All say "Me".
    'Undo' is the simplest form of version control.

*   Give learners a five-minute overview of what version control does for them
    before diving into the watch-and-do practicals.
    Most of them will have tried to co-author papers by emailing files back and forth,
    or will have biked into the office only to realize that the USB key with last night's work is still on the kitchen table.
    Instructors can also make jokes about directories with names like "final version",
    "final version revised",
    "final version with reviewer three's corrections",
    "really final version",
    and,
    "come on this really has to be the last version" to motivate version control as a better way to collaborate and as a better way to back work up.

**Setting Up Mercurial**

*   We suggest instructors and students use `nano` as the text editor for this lessons because

    - it runs in all three major operating systems,
    - it runs inside the shell (switching windows can be confusing to students), and
    - it has shortcut help at the bottom of the window.

    Please point out to students during setup that they can and should use another text editor if they're already familiar with it.

*   When setting up Mercurial,
    be very clear what learners have to enter:
    it is common for them to edit the instructor's details
    (e.g. email).


**Creating a Repository** and **Changing a File**

*   When you do `hg status`,
    Mac users may see a `.DS_Store` file showing as untracked.
    This a file that Mac OS creates in each directory.


**Tracking Changes to Files**

*   It's important that learners do a full commit cycle by themselves
    (make changes, `hg diff`, and `hg commit`).
    The "`bio` repository" challenge does that.

*   This is a good moment to show a diff with the KDiff3 graphical diff tool.
    If you skip it because you're short on time,
    show the graphical diff view of a commit on Bitbucket in the [Collaborating](02-collab.html) section.


**Exploring History** and **Recovering Old Versions**

*   Mention that you can revert more than one file at a time by listing the file names one after the other in the `hg revert` command,
and that shell wildcard characters can also be used.


## [Collaborating](02-collab.html)

FIXME

## [Conflicts](03-conflict.html)

FIXME

## [Open Science](04-open.html)

FIXME
