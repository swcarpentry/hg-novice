---
title: Tracking Changes to Files
teaching: 10
exercises: 5
questions:
- "How do I record changes in Mercurial?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"
objectives:
- "Display the version control status of files in a repository and explain what those statuses mean."
- "Add files to Mercurial's collection of tracked files."
- "Record metadata about changes to a file."
- "Display the history of changes to files in a repository and explain the metadata that is recorded with each changeset."
keypoints:
- "`hg status` shows the status of a repository."
- "`hg add` puts files in the staging area."
- "`hg commit` saves the staged content as a new commit in the local repository."
- "Always write a log message when committing changes."
---

Let's create a file called `plan.txt` in which Susan is going to write her
initial ideas and notes about the Salish Sea NEMO daily forecast system.
You can use any text editor you want
(`Notepad`, `TextEdit`, `gedit`, `nano`, `emacs`, `vi`, ...)
but please don't use a word processor like Microsoft Word or LibreOffice Write.
In particular,
the editor does not have to be the one that you set in your Mercurial
configuration earlier.
In what follows it is assumed that `nano` is used.

~~~
$ nano plan.txt
~~~
{: .bash}

and type into that file:

~~~
Goal: Run NEMO everyday to forecast storm surge water levels
~~~
{: .output}

`plan.txt` has now been created and it contains a single line:

~~~
$ ls
~~~
{: .bash}

~~~
plan.txt
~~~
{: .output}

~~~
$ cat plan.txt
~~~
{: .bash}

~~~
Goal: Run NEMO everyday to forecast storm surge water levels
~~~
{: .output}

We can ask Mercurial to tell us what it knows about the files in our project
with the `hg status` command.
Mercurial tells us that it has noticed the new file:

~~~
$ hg status
~~~
{: .bash}

~~~
? plan.txt
~~~
{: .output}

The `?` at the beginning of the line means that Mercurial isn't keeping track
of the file.
We can tell Mercurial that it should do so using `hg add`:

~~~
$ hg add plan.txt
~~~
{: .bash}

and then check that the right thing happened:

~~~
$ hg status
~~~
{: .bash}

~~~
A plan.txt
~~~
{: .output}

Mercurial now knows that it's supposed to keep track of `plan.txt`,
but it hasn't yet recorded any changes for posterity as a commit.
To get it to do that,
we need to run one more command:

~~~
$ hg commit -m "Starting to plan the daily NEMO forecast system."
~~~
{: .bash}

When we run `hg commit`,
Mercurial takes the file we have told it about by using `hg add` and stores
a copy permanently inside the special `.hg` directory.
That permanent copy is called a [commit]({{ page.root }}/reference/#commit)
(or [revision]({{ page.root }}/reference/#revision)).

We use the `-m` flag (for "message") to record a comment that will help us
remember later on what we did and why.
If we just run `hg commit` without the `-m` option,
Mercurial will launch `nano`
(or whatever other editor we configured at the start)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<70 characters)
summary of changes made in the commit.
If you want to go into more detail,
add a blank line between the summary line and your additional notes.

[commit-messages]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

If we run `hg status` now:

~~~
$ hg status
~~~
{: .bash}

we get no output because everything is up to date.

If we want to know what we've done recently,
we can ask Mercurial to show us the project's history using `hg log`:

~~~
$ hg log
~~~
{: .bash}

~~~
changeset:   0:1320339bbcae
tag:         tip
user:        Susan Allen <sallen@eos.ubc.ca>
date:        Tue Jun 09 14:41:27 2015 +0200
summary:     Starting to plan the daily NEMO forecast system.

~~~
{: .output}

`hg log` lists all changes committed to a repository,
starting with the most recent.
The listing for each [changeset]({{ page.root }}/reference/#changeset) includes:

* the changeset's revision number and identifier
  (`0` and `1320339bbcae` in this case,
  but your identifier will likely be different),
* its tags
  (more about tags later),
* the changeset's author (`user`),
* when it was created,
* and the log message Mercurial was given when the changeset was created.

The revision number is a convenient integer shorthand for the hexadecimal
identifier.

> ## Where Are My Changes?
>
> If we run `ls` at this point,
> we will still see just one file called `plan.txt`.
> That's because Mercurial saves information about files' history in the
> special `.hg` directory mentioned earlier so that our filesystem doesn't
> become cluttered
> (and so that we can't accidentally edit or delete an old version).
{: .callout}
