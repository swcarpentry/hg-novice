---
title: Creating a Repository
teaching: 10
exercises: 5
questions:
- "Where does Mercurial store information?"
objectives:
- "Explain how to initialize a new Mercurial repository."
keypoints:
- "`hg init` initializes a repository."
---

We'll start by exploring how version control can be used to keep track of what
one person did and when.
We're going to pretend to be a researcher named Susan who wants to start running
the Salish Sea NEMO ocean model as a daily forecast system.
If the example in the next few sections seems contrived,
be assured that it is only slightly so to avoid having to do too much typing.
Lots of projects
(and therefore version control repositories)
start with a single text file in which someone writes down some initial ideas.

Now that we have Mercurial [configured]({{ page.root }}/02-configuration/),
we can start using it.
Let's create a directory for Susan's NEMO forecast project:

~~~
$ mkdir forecast
$ cd forecast
~~~
{: .bash}

and tell Mercurial to make it a [repository]({{ page.root }}/reference/#repository):
a place where Mercurial can store versions of our files:

~~~
$ hg init
~~~
{: .bash}

Mercurial commands are written `hg verb`,
where `verb` is what we actually want it to do.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{: .bash}

But if we add the `-a` flag to show everything,
we can see that Mercurial has created a hidden directory within `forecast`
called `.hg`:

~~~
$ ls -a
~~~
{: .bash}

~~~
. ..  .hg
~~~
{: .output}

Mercurial stores information about the project in this special sub-directory.
If we ever delete it,
we will lose the project's history.

We can check that everything is set up correctly
by asking Mercurial to verify the structure of our repository:

~~~
$ hg verify
~~~
{: .bash}

~~~
checking changesets
checking manifests
crosschecking files in changesets and manifests
checking files
0 files, 0 changesets, 0 total revisions
~~~
{: .output}
