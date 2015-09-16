---
layout: page
title: Version Control with Mercurial
subtitle: Ignoring Things
---
> ## Learning Objectives {.objectives}
>
> *   Configure Mercurial to ignore specific files and explain why it is
>     sometimes useful to do so.

What if we have files that we do not want Mercurial to track for us,
like backup files created by our editor or intermediate files created during
data analysis?
Let's create a few dummy files:

~~~ {.bash}
$ mkdir inprogress
$ touch plan.txt~ inprogress/a.out inprogress/b.out
~~~

and see what Mercurial says:

~~~ {.bash}
$ hg status
~~~
~~~ {.output}
? plan.txt~
? inprogress/a.out
? inprogress/b.out
~~~

Putting these files under version control would be a waste of disk space.
What's worse,
having them all listed could distract us from changes that actually matter,
so let's tell Mercurial to ignore them.

We do this by creating a file in the root directory of our project called
`.hgignore`.

~~~ {.bash}
$ nano .hgignore
$ cat .hgignore
~~~
~~~ {.output}
syntax: glob
*~
inprogress/
~~~

The `syntax: glob` line at the top of the file tells Mercurial that we want to
use the same kind of pattern matching that we use in the shell
(which is known as "globbing" and the patterns as "globs").
The second line tells Mercurial to ignore any file whose name ends with `~`
and the third one to ignore everything in the `inprogress` directory.
(If any of these files were already being tracked,
Mercurial would continue to track them.)

Once we have created this file,
the output of `hg status` is much cleaner:

~~~ {.bash}
$ hg status
~~~
~~~ {.output}
? .hgignore
~~~

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

~~~ {.bash}
$ hg add .hgignore
$ hg commit -m "Don't track editor backup or in-progress analysis files."
$ hg status
~~~

We can also always see the status of ignored files if we want:

~~~ {.bash}
$ hg status --ignored
~~~
~~~ {.output}
I plan.txt~
I inprogress/a.out
I inprogress/b.out
~~~
