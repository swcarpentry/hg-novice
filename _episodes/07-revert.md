---
title: Recovering Old Versions
teaching: 10
exercises: 5
questions:
- "How can I recover old versions of files?"
objectives:
- "Restore older versions of files."
- "Use configuration aliases to create custom Mercurial commands."
keypoints:
- "Use `hg revert` to recover old versions of files."
---

All right:
we can save changes to files and see what we've changed—how can we restore older versions of things?
Let's suppose we (somehow) accidentally overwrite Salish Sea NEMO forecast
planning file with our grocery list:

~~~
$ nano plan.txt
$ cat plan.txt
~~~
{: .bash}

~~~
Ricotta
Mushroom Tortellini
Bacon
~~~
{: .output}

`hg status` now tells us that the file has been changed,
but those changes haven't been committed:

~~~
$ hg status
~~~
{: .bash}

~~~
M plan.txt
~~~
{: .output}

We can put things back the way they were by using `hg revert`:

~~~
$ hg revert plan.txt
$ cat plan.txt
~~~
{: .bash}

~~~
Goal: Run NEMO everyday to forecast storm surge water levels

Need daily high resolution weather forcing from EC.
Also need daily average Fraser River flow from EC.
~~~
{: .output}

As you might guess from its name,
`hg revert` reverts to (i.e. restores) an old version of a file.
In this case,
we're telling Mercurial that we want to recover the last committed version
of the file.
If we want to go back even further,
we can use the `--rev` or `-r` flag and a revision number instead:

~~~
$ hg revert --rev 0 plan.txt
~~~
{: .bash}

Mercurial really doesn't want to cause us to lose our work,
so it defaults to making a backup when we use `hg revert`:

~~~
$ hg status
~~~
{: .bash}

~~~
? plan.txt.orig
~~~
{: .output}

The `plan.txt.orig` file is a copy of `plan.txt` as it stood before the
`hg revert` command.
It's not tracked by Mercurial.
It's just there in case we made a mistake and really didn't want to revert,
or in case there's some content from before the revert that we decide that
we really do want to copy into `plan.txt`.
When we're sure that we don't need `*.orig` files we can just go ahead and
delete them.
If we really don't want Mercurial to create `*.orig` files when we use
`hg revert`,
we can use the `--no-backup` option,
or its short version `-C`.

The fact that files can be reverted one by one tends to change the way people
organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction without
also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.

> ## Using Aliases to Create New Mercurial Commands
>
> If we decide that reverting without creating `.orig` backup file is something
> that we want to do regularly we can use `hg config --edit` to add
>
> ~~~
> [alias]
> nb-revert = revert --no-backup
> ~~~
> {: .source}
>
> to our Mercurial configuration settings.
> After we save the configuration file and exit from the editor we have a new
> command,
> `hg nb-revert`
> (no backup revert) that is the same as typing `hg revert --no-backup`.
> Note that it is good practice when creating aliases for commands to give them
> your own names instead of using the alias to redefine the built-in command.
> If we had used the alias to change add the `--no-backup` flag to the `revert`
> command
>
> ~~~
> [alias]
> revert = revert --no-backup
> ~~~
> {: .source}
>
> it would be difficult to get `hg revert` to create `.orig` backup files if we
> ever wanted it to.
{: .callout}
