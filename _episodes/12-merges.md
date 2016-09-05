---
title: Merging Changes from Different Clones
teaching: 10
exercises: 5
questions:
- "How do I combine changes made in different repositories?"
objectives:
- "Explain how Mercurial handles changes that make a repository's history diverge."
- "Explain what merges are."
keypoints:
- "Use `hg merge` to combine independent changes to the same set of files."
---

As soon as people can work in parallel,
someone's going to step on someone else's toes.
This will even happen with a single person:
if we are working on a piece of software on both our laptop and a workstation in the lab,
we could make different changes to each copy.
Version control helps us manage these divergent changes by merging many of them automatically.

To see how automatic merging works let's go back to the `home` and `work` clones of Susan's `forecast` repo.

Let's add another line to the `plan.txt` file in our `home` clone to that it contains:

~~~
Goal: Run NEMO everyday to forecast storm surge water levels

Need daily high resolution weather forcing from EC.
Also need daily average Fraser River flow from EC.
Use the NOAA Neah Bay sea surface height anomaly forecast and observations for the western boundary forcing.
~~~
{: .output}

then commit the change and push it to Bitbucket:

~~~
$ hg ci -m"Add a line in our home clone."
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
remote: adding changesets
remote: adding manifests
remote: adding file changes
remote: added 1 changesets with 1 changes to 1 files
~~~
{: .output}

Now let's switch to the `work` clone and make a different change there *without* updating from Bitbucket first.
We'll make the goal of our project more ambitious so that the `plan.txt` file looks like:

~~~
Goal: Run NEMO 3 times every day to forecast storm surge water levels

Need daily high resolution weather forcing from EC.
Also need daily average Fraser River flow from EC.
~~~
{: .output}

We can commit the change locally:

~~~
$ hg commit -m "Change a line in the work clone."
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
remote has heads on branch 'default' that are not known locally: ce4056d28038
abort: push creates new remote head e0747e3feea1!
(pull and merge or see "hg help push" for details about pushing new heads)
~~~
{: .output}

Mercurial detects that our changes have diverged with the changes in the remote
repo on Bitbucket and stops us from creating a confusing situation on there.
What we have to do is pull the changes from Bitbucket,
[merge]({{ page.root }}/reference/#merge) them into the copy we're currently working in,
and then push that.
Let's start by pulling:

~~~
$ hg pull
~~~
{: .bash}

~~~
pulling from https://bitbucket.org/susan/forecast
searching for changes
adding changesets
adding manifests
adding file changes
added 1 changesets with 1 changes to 1 files (+1 heads)
(run 'hg heads' to see heads, 'hg merge' to merge)
~~~
{: .output}

`hg pull` tells us there's now an extra head in our repo and suggests that we use
`hg heads` to inspect the situation and `hg merge` to resolve it.
Let's first attempt to understand the situation:

~~~
$ hg heads
~~~
{: .bash}

~~~
changeset:   5:ce4056d28038
tag:         tip
parent:      3:68fd235a0541
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Sun Jun 14 13:07:22 2015 -0700
summary:     Add a line in our home clone.

changeset:   4:e0747e3feea1
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Sun Jun 14 13:16:14 2015 -0700
summary:     Change a line in the work clone.
~~~
{: .output}

This shows us that our repository currently has two heads.
Looking at the graph of our log with `hg log --graph` or `hg log -G` may be more
informative:

~~~
$ hg log -G
~~~
{: .bash}

~~~
o  changeset:   5:ce4056d28038
|  tag:         tip
|  parent:      3:68fd235a0541
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 13:07:22 2015 -0700
|  summary:     Add a line in our home clone.
|
| @  changeset:   4:e0747e3feea1
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

With the graph,
we can see more details at a glance.
The `@` marker shows the commit we're currently standing on,
`4:e0747e3feea1`.
We have just pulled changeset `5:ce4056d28038`,
which added a head to our clone,
because it was also based off `3:68fd235a0541`.
Our repository history has diverged;
i.e. two different changesets have branched off from the same parent commit.

It is important to note something about revision numbers here
(the `4` and the `5` before the `:`).
The same commits may have different revision numbers in different clones.
These revision numbers are assigned in the order that a clone acquires new commits.
On the other hand,
the identifiers
(`e0747e3feea1` and `ce4056d28038` in this case)
uniquely identify the changeset no matter in which clone it is.
When we communicate with other people,
it is important to use identifiers.
When we work alone,
however,
we may use revision numbers as a convenience.

Now that we understand the situation,
let's resolve it as Mercurial suggested when we pulled,
with the `hg merge` command:

~~~
$ hg merge
~~~
{: .bash}

~~~
merging plan.txt
0 files updated, 1 files merged, 0 files removed, 0 files unresolved
(branch merge, don't forget to commit)
~~~
{: .output}

Mercurial has merged the changes that we have to the `plan.txt` file in the different clones of the repo and is helpfully reminding us that we still need to commit the result of that merge.
Before we do that,
let's examine the situation to understand the present state of the repo:

~~~
$ hg status
~~~
{: .bash}

~~~
M plan.txt
~~~
{: .output}

The `plan.txt` file has been modified.

~~~
$ hg diff
~~~
{: .bash}

~~~
diff -r e0747e3feea1 plan.txt
--- a/plan.txt  Sun Jun 14 13:16:14 2015 -0700
+++ b/plan.txt  Sun Jun 14 13:43:15 2015 -0700
@@ -1,4 +1,5 @@
 Goal: Run NEMO 3 times every day to forecast storm surge water levels

 Need daily high resolution weather forcing from EC.
 Also need daily average Fraser River flow from EC.
+Use the NOAA Neah Bay sea surface height anomaly forecast and observations for the western boundary forcing.
~~~
{: .output}

The diff shows the changed goal that we did in the `work` clone,
and the line about sea surface height forcing that we added in the `home` clone.

~~~
$ hg summary
~~~
{: .bash}

~~~
parent: 4:e0747e3feea1
 Change a line in the work clone.
parent: 5:ce4056d28038 tip
 Add a line in our home clone.
branch: default
commit: 1 modified (merge)
update: (current)
~~~
{: .output}

This tells us that our working directory is in a merged state.
There are two parents,
and one file that has been merged and it ready to be committed.

~~~
$ hg log -G
~~~
{: .bash}

~~~
@  changeset:   5:ce4056d28038
|  tag:         tip
|  parent:      3:68fd235a0541
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 13:07:22 2015 -0700
|  summary:     Add a line in our home clone.
|
| @  changeset:   4:e0747e3feea1
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

The `@` marker appears in *two* locations.
This indicates that our working directory is in a merged state.

We've looked at things from a lot of different angles now,
so let's finalize the merge by committing it:

~~~
$ hg commit -m "Merge changes from home."
~~~
{: .bash}

and take one more look at the graph:

~~~
$ hg log -G
~~~
{: .bash}

~~~
@    changeset:   6:d6d860a02aef
|\   tag:         tip
| |  parent:      4:e0747e3feea1
| |  parent:      5:ce4056d28038
| |  user:        Doug Latornell <djl@douglatornell.ca>
| |  date:        Sun Jun 14 13:52:57 2015 -0700
| |  summary:     Merge changes from home.
| |
| o  changeset:   5:ce4056d28038
| |  parent:      3:68fd235a0541
| |  user:        Doug Latornell <djl@douglatornell.ca>
| |  date:        Sun Jun 14 13:07:22 2015 -0700
| |  summary:     Add a line in our home clone.
| |
o |  changeset:   4:e0747e3feea1
|/   user:        Doug Latornell <djl@douglatornell.ca>
|    date:        Sun Jun 14 13:16:14 2015 -0700
|    summary:     Change a line in the work clone.
|
o  changeset:   3:68fd235a0541
|  user:        Doug Latornell <djl@douglatornell.ca>
|  date:        Sun Jun 14 11:04:44 2015 -0700
|  summary:     Start creating bibliography for the forecast paper.
|
o  changeset:   2:d14b5f2372fc
|  user:        Doug Latornell <djl@douglatornell.ca>
|  date:        Sun Jun 14 11:03:17 2015 -0700
|  summary:     Add note about data source for Fraser River flow forcing.
|
o  changeset:   1:41b3e1218ad2
|  user:        Doug Latornell <djl@douglatornell.ca>
|  date:        Sun Jun 14 11:02:05 2015 -0700
|  summary:     Add note about source for atmospheric forcing.
|
o  changeset:   0:983898576cad
   user:        Doug Latornell <djl@douglatornell.ca>
   date:        Sun Jun 14 11:00:20 2015 -0700
   summary:     Starting to plan the daily NEMO forecast system.

~~~
{: .output}

which shows us that the divergence has been merged and our repo is back to having one head.
Now,
finally,
we can push the changes to Bitbucket:

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
remote: adding changesets
remote: adding manifests
remote: adding file changes
remote: added 2 changesets with 2 changes to 1 files
~~~
{: .output}

It should be noted that we could have avoided the merge altogether if we had pulled and updated with the changes from the `home` clone before we started making changes in the `work` clone.
That highlights a couple of best practices for using version control:

* Make commits often,
for small,
easy to describe pieces of work.
* Push your changes to Bitbucket
(or whatever hub repository you use)
before switching from one machine to another.
* Pull and update before you start making changes.

Doing those things will virtually eliminate merges in repositories where you are working by yourself.
In repositories where you are collaborating with other people merges are a fact of life,
but the rules above can help reduce the number of merges and make them less traumatic.
