---
title: Making Changes
teaching: 10
exercises: 5
questions:
- "How can I change files in a repository?"
objectives:
- "Display the uncommitted changes that have been made to tracked files."
- "Go through the modify-commit cycle for single and multiple files."
keypoints:
- "`hg diff` displays differences between commits."
- "`hg log` shows the history of a repository."
---

Now suppose Susan adds more notes to the file.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

~~~
$ nano plan.txt
$ cat plan.txt
~~~
{: .bash}

~~~
Goal: Run NEMO everyday to forecast storm surge water levels

Need daily high resolution weather forcing from Environment Canada.
~~~
{: .output}

When we run `hg status` now,
it tells us that a file it already knows about has been modified:

~~~
$ hg status
~~~
{: .bash}

~~~
M plan.txt
~~~
{: .output}

The `M` at the beginning of the line means that Mercurial has noticed that
we have modified the `plan.txt` file.

We can double-check our work using `hg diff`,
which shows us the differences between the current state of the file and the
most recently committed version:

~~~
$ hg diff
~~~
{: .bash}

~~~
diff -r 1320339bbcae plan.txt
--- a/plan.txt  Tue Jun 09 14:41:27 2015 +0200
+++ b/plan.txt  Tue Jun 09 15:07:42 2015 +0200
@@ -1,1 +1,3 @@
 Goal: Run NEMO everyday to forecast storm surge water levels
+
+Need daily high resolution weather forcing from Environment Canada.
~~~
{: .output}

The output is cryptic because it is actually a series of commands for tools like
editors and `patch` telling them how to reconstruct one file given the other.
If we can break it down into pieces:

1.  The first line tells us that Mercurial is using the Unix `diff` command to
    compare the last committed and new versions of the file.

2.  The next two lines show us the time stamps of the 2 versions of the file
    that are being compared.
3.  The remaining lines show us the actual differences and the lines on which
    they occur.
    In particular,
    the `+` markers in the first column show where we are adding lines.

Let's commit our change:

~~~
$ hg commit -m "Add note about source for atmospheric forcing."
~~~
{: .bash}

Checking our project's status:

~~~
$ hg status
~~~
{: .bash}

we get no output because all of the changes have been committed.
We can see our commits with `hg log`:

~~~
$ hg log
~~~
{: .bash}

~~~
changeset:   1:b31241913818
tag:         tip
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Tue Jun 09 15:16:11 2015 +0200
summary:     Add note about source for atmospheric forcing.

changeset:   0:1320339bbcae
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Tue Jun 09 14:41:27 2015 +0200
summary:     Starting to plan the daily NEMO forecast system.

~~~
{: .output}

> ## Committing Only Some of the Changes
>
> Of course sometimes we may not want to commit everything at once.
> For example,
> suppose we're adding a few citations to our supervisor's work to our thesis.
> We might want to commit those additions,
> and the corresponding addition to the bibliography,
> but *not* commit the work we're doing on the conclusions
> (which we haven't finished yet).
> To handle that,
> simply do two
> (or more)
> separate commits,
> listing the names of the files to be included in each commit in the `hg commit`
> command:
>
> ~~~
> $ hg commit -m "Cite Sastri and Dower (2009)." methods.txt biblio.txt
> ...
> <later>
> ...
> $ hg commit conclusions.txt -m "Update conclusions re: phyto bloom timing mismatches."
> ~~~
> {: .bash}
>
> Notice that the list of file names can come before or after the commit comment
> in the `hg commit` command.
{: .callout}

Let's add another line to the file for practice and to make our revision
history more interesting:

~~~
$ nano plan.txt
$ cat plan.txt
~~~
{: .bash}

~~~
Goal: Run NEMO everyday to forecast storm surge water levels

Need daily high resolution weather forcing from Environment Canada.
Also need daily average Fraser River flow from Environment Canada.
~~~
{: .output}

~~~
$ hg diff
~~~
{: .bash}

~~~
diff -r b31241913818 plan.txt
--- a/plan.txt  Tue Jun 09 15:16:11 2015 +0200
+++ b/plan.txt  Tue Jun 09 15:25:33 2015 +0200
@@ -1,3 +1,4 @@
 Goal: Run NEMO everyday to forecast storm surge water levels

 Need daily high resolution weather forcing from Environment Canada.
+Also need daily average Fraser River flow from Environment Canada.
~~~
{: .output}

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column).
Now,
let's commit our changes:

~~~
$ hg commit plan.txt -m "Add note about data source for Fraser River flow forcing."
~~~
{: .bash}

and look at the history of what we've done so far:

~~~
$ hg log
~~~
{: .bash}

~~~
changeset:   2:2e15a7ee29c2
tag:         tip
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Tue Jun 09 15:28:25 2015 +0200
summary:     Add note about data source for Fraser River flow forcing.

changeset:   1:b31241913818
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Tue Jun 09 15:16:11 2015 +0200
summary:     Add note about source for atmospheric forcing.

changeset:   0:1320339bbcae
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Tue Jun 09 14:41:27 2015 +0200
summary:     Starting to plan the daily NEMO forecast system.

~~~
{: .output}

> ## `bio` Repository
>
> Create a new Mercurial repository on your computer called `bio`.
> Write a three-line biography for yourself in a file called `me.txt`,
> commit your changes,
> then modify one line,
> add a fourth line,
> and display the differences between the file's updated state and its
> original state.
{: .challenge}
