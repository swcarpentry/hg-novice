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

*   The [Conflicts](03-conflict.heml) section of the lesson uses the [KDiff3](http://kdiff3.sourceforge.net/) graphical diff/merge tool.
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

FIXME

## [Collaborating](02-collab.html)

FIXME

## [Conflicts](03-conflict.html)

FIXME

## [Open Science](04-open.html)

FIXME
