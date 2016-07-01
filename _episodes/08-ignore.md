---
title: Ignoring Things
teaching: 10
exercises: 5
questions:
- "How can I tell Mercurial to ignore files I don't want to track?"
objectives:
- "Configure Mercurial to ignore specific files and explain why it is sometimes useful to do so."
keypoints:
- "Create a `.hgignore` file to specify things that Mercurial should ignore."
---

What if we have files that we do not want Mercurial to track for us,
like backup files created by our editor or intermediate files created during
data analysis?
Let's create a few dummy files:

~~~
$ mkdir inprogress
$ touch plan.txt~ inprogress/a.out inprogress/b.out
~~~
{: .bash}

and see what Mercurial says:

~~~
$ hg status
~~~
{: .bash}

~~~
? plan.txt~
? inprogress/a.out
? inprogress/b.out
~~~
{: .output}

Putting these files under version control would be a waste of disk space.
What's worse,
having them all listed could distract us from changes that actually matter,
so let's tell Mercurial to ignore them.

We do this by creating a file in the root directory of our project called
`.hgignore`.

~~~
$ nano .hgignore
$ cat .hgignore
~~~
{: .bash}

~~~
syntax: glob
*~
inprogress/
~~~
{: .output}

The `syntax: glob` line at the top of the file tells Mercurial that we want to
use the same kind of pattern matching that we use in the shell
(which is known as "globbing" and the patterns as "globs").
The second line tells Mercurial to ignore any file whose name ends with `~`
and the third one to ignore everything in the `inprogress` directory.
(If any of these files were already being tracked,
Mercurial would continue to track them.)

Once we have created this file,
the output of `hg status` is much cleaner:

~~~
$ hg status
~~~
{: .bash}

~~~
? .hgignore
~~~
{: .output}

The only thing Mercurial notices now is the newly-created `.hgignore` file.
You might think we wouldn't want to track it,
but which files we're tracking,
which ones we're not,
when we made those decisions,
and why is part of the story of our project's development that will help us
understand the project weeks,
months,
or years in the future.
So,
let's add and commit `.hgignore`:

~~~
$ hg add .hgignore
$ hg commit -m "Don't track editor backup or in-progress analysis files."
$ hg status
~~~
{: .bash}

We can also always see the status of ignored files if we want:

~~~
$ hg status --ignored
~~~
{: .bash}

~~~
I plan.txt~
I inprogress/a.out
I inprogress/b.out
~~~
{: .output}
