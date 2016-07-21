---
title: Merge Conflicts
teaching: 10
exercises: 5
questions:
- "What do I do when my changes conflict with someone else's?"
objectives:
- "Explain what merge conflicts are and when they can occur."
- "Resolve conflicts resulting from a merge using the KDiff3 tool."
keypoints:
- "Conflicts occur when two or more people change the same file(s) at the same time."
- "The version control system does not allow people to overwrite each other's changes blindly, but highlights conflicts so that they can be resolved."
---

When Mercurial can't decide how to merge changes automatically it asks us to
resolve the conflicts that it has detected,
and it provides us ways to use other software tools,
like [KDiff3](http://kdiff3.sourceforge.net/),
to help us visualize the conflicting changes and decide how to resolve them.

What happens if we edit the same line in a file in 2 different clones?

Let's go back to the `home` clone and make another change.
First though,
we'll check to see if there are any incoming changes that we should pull and
update before we start working:

~~~
$ hg incoming
~~~
{: .bash}

~~~
comparing with https://bitbucket.org/susan/forecast
searching for changes
http authorization required for https://bitbucket.org/susan/forecast
realm: Bitbucket.org HTTP
user: susan
password:
changeset:   5:e0747e3feea1
parent:      3:68fd235a0541
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Sun Jun 14 13:16:14 2015 -0700
summary:     Change a line in the work clone.

changeset:   6:d6d860a02aef
tag:         tip
parent:      5:e0747e3feea1
parent:      4:ce4056d28038
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Sun Jun 14 13:52:57 2015 -0700
summary:     Merge changes from home.

~~~
{: .output}

Those are the the result of our work in the last section on automatic merges.

Pulling and updating:

~~~
$ hg pull
~~~
{: .bash}

~~~
pulling from https://bitbucket.org/susan/forecast
searching for changes
http authorization required for https://bitbucket.org/susan/forecast
realm: Bitbucket.org HTTP
user: susan
password:
adding changesets
adding manifests
adding file changes
added 2 changesets with 2 changes to 1 files
(run 'hg update' to get a working copy)
~~~
{: .output}

~~~
$ hg update
~~~
{: .bash}

~~~
1 files updated, 0 files merged, 0 files removed, 0 files unresolved
~~~
{: .output}

Adding another line to the `plan.txt` file in our `home` clone so that it contains:

~~~
Goal: Run NEMO everyday to forecast storm surge water levels

Need daily high resolution weather forcing from EC.
Also need daily average Fraser River flow from EC.
Use the NOAA Neah Bay sea surface height anomaly forecast and observations for the western boundary forcing.

Run the model on orcinus.
~~~
{: .output}

then commit the change and push it to Bitbucket:

~~~
$ hg ci -m"Add a line on home about where we'll run the model."
$ hg push
~~~
{: .bash}

Switch to the `work` clone and make a different change to the same line there
*without* updating from Bitbucket first.

~~~
Goal: Run NEMO 3 times every day to forecast storm surge water levels

Need daily high resolution weather forcing from EC.
Also need daily average Fraser River flow from EC.

Run the model on west.cloud.
~~~
{: .output}

We can commit the change locally:

~~~
$ hg ci -m"Add a different line on work about where we'll run the model."
~~~
{: .bash}

but Mercurial won't let us push it to Bitbucket:

~~~
$ hg push
~~~
{: .bash}

~~~
pushing to https://bitbucket.org/susan/forecast
searching for changes
http authorization required for https://bitbucket.org/susan/forecast
realm: Bitbucket.org HTTP
user: susan
password:
remote has heads on branch 'default' that are not known locally: bd73857a6aea
abort: push creates new remote head ba1615cd23ae!
(pull and merge or see "hg help push" for details about pushing new heads)
~~~
{: .output}

Once again,
Mercurial has detected the divergence of our repo's history and insists that we
resolve the matter locally before we push to Bitbucket.

Pulling the changes,
and looking at the log graph we see:

~~~
$ hg pull
~~~
{: .bash}

~~~
pulling from https://bitbucket.org/susan/forecast
searching for changes
http authorization required for https://bitbucket.org/susan/forecast
realm: Bitbucket.org HTTP
user: susan
password:
adding changesets
adding manifests
adding file changes
added 1 changesets with 1 changes to 1 files (+1 heads)
(run 'hg heads' to see heads, 'hg merge' to merge)
~~~
{: .output}

~~~
$ hg log --graph
~~~
{: .bash}

~~~
o  changeset:   8:bd73857a6aea
|  tag:         tip
|  parent:      6:d6d860a02aef
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 14:28:57 2015 -0700
|  summary:     Add a line on home about where we'll run the model.
|
| @  changeset:   7:ba1615cd23ae
|/   user:        Susan Allen <sallen@eos.ubc.ca>
|    date:        Sun Jun 14 14:45:47 2015 -0700
|    summary:     Add a different line on work about where we'll run the model.
|
o    changeset:   6:d6d860a02aef
|\   parent:      4:e0747e3feea1
| |  parent:      5:ce4056d28038
| |  user:        Susan Allen <sallen@eos.ubc.ca>
| |  date:        Sun Jun 14 13:52:57 2015 -0700
| |  summary:     Merge changes from home.
| |
| o  changeset:   5:ce4056d28038
| |  parent:      3:68fd235a0541
| |  user:        Susan Allen <sallen@eos.ubc.ca>
| |  date:        Sun Jun 14 13:07:22 2015 -0700
| |  summary:     Add a line in our home clone.
| |
o |  changeset:   4:e0747e3feea1
|/   user:        Susan Allen <sallen@eos.ubc.ca>
|    date:        Sun Jun 14 13:16:14 2015 -0700
|    summary:     Change a line in the work clone.
|
o  changeset:   3:68fd235a0541
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 11:04:44 2015 -0700
|  summary:     Start creating bibliography for the forecast paper.
|
o  changeset:   2:d14b5f2372fc
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 11:03:17 2015 -0700
|  summary:     Add note about data source for Fraser River flow forcing.
|
o  changeset:   1:41b3e1218ad2
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 11:02:05 2015 -0700
|  summary:     Add note about source for atmospheric forcing.
|
o  changeset:   0:983898576cad
   user:        Susan Allen <sallen@eos.ubc.ca>
   date:        Sun Jun 14 11:00:20 2015 -0700
   summary:     Starting to plan the daily NEMO forecast system.

~~~
{: .output}

The commit messages on changesets `7` and `8` are a dead giveaway that there will
be a problem when we try to merge.
We can also look at the local changes with `hg diff --rev 8` and the changes that
we are pulling using the `Commits` view on Bitbucket.
Or,
we can just be pessimistic about merges and always assume that there will be
conflicts that we will want to use a visualization tool to help us resolve.
Whatever the case,
we tell Mercurial to use the `kdiff3` tool to help us through the merge:

~~~
$ hg merge --tool=kdiff3
~~~
{: .bash}

~~~
merging plan.txt
~~~
{: .output}

It may take a few seconds for Mercurial to pass the necessary information
to `kdiff3`,
but soon you should see a window that looks like this:

![KDiff3 Merge Conflict](../fig/hg-kdiff3-conflict.png)

The pane labelled `base` refers to this file's state in changeset `6`,
the parent changeset of our conflicting changesets `7` and `8`.
Since we were standing on `8` when we started the merge,
that is the pane labelled `local`.
We're attempting to merge this with changeset `7`,
which is labelled `other`.

kdiff3 has buttons for moving from one conflict to another.
In this case,
there is only one conflict.
At each conflict we can select either the `local` or the `other` change or to write something completely different in the `Output` pane at the bottom.

It is now up to us to edit this file to reconcile the changes.
We can do anything we want:

* Keep the change from our local branch by clicking the `B` button in the `kdiff3` toolbar,
* Keep the change pulled from the other clone by clicking on the `C` button in the toolbar,
* Get rid of the change entirely by clicking on the `A` button in the toolbar to effectively revert to the parent changeset, or
* Write something new to replace both the local and pulled change.

Let's do the latter and resolve the conflict so it looks like:

~~~
Run the model on orcinus or west.cloud.
~~~
{: .output}

When we save the merge result in `kdiff3` and exit,
Mercurial tells us that:

~~~
0 files updated, 1 files merged, 0 files removed, 0 files unresolved
(branch merge, don't forget to commit)
~~~
{: .output}

indicating that the merge has been successful and is ready to be completed.
We can use any or all of the same commands that we used after the automatic merge
in the previous section to inspect the results of the merge:

* `hg status`
* `hg diff`
* `hg summary`
* `hg log --graph`

In the end,
we finalize the merge by committing it and pushing it to Bitbucket:

~~~
$ hg commit -m "Merge changes regarding where we'll run the model."
$ hg push
~~~
{: .bash}

Merge conflicts are generally uncommon when you are working by yourself in a
repository.
They are also fairly rare when multiple people are collaborating on plain text
files like code,
LaTeX documents,
or web page content.
