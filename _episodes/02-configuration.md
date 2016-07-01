---
title: Configuring Mercurial
teaching: 10
exercises: 5
questions:
- "How do I get set up to use Mercurial?"
objectives:
- "Explain what configuration steps are required the first time Mercurial is used on a computer."
keypoints:
-   "Use `hg config` to configure a user name, email address, editor, and other preferences once per machine."
---

When we use [Mercurial](http://mercurial.selenic.com/) on a computer for the
first time,
we need to configure a few things.
The command

~~~
$ hg config --edit
~~~
{: .bash}

should open a template Mercurial configuration file in an editor for you.
On Windows the file will likely appear in `Notepad`.
On OS X and Linux,
if your `EDITOR` environment variable is not set to something else,
the file will likely appear in `vi`.
If that doesn't make you happy,
prefix the command with `EDITOR=<editor-of-your-choice>`.
`nano` is a nice,
safe choice if you don't know what else to choose:

~~~
$ EDITOR=nano hg config --edit
~~~
{: .bash}

The file should look a lot like:

~~~
# example user config (see "hg help config" for more info)
[ui]
# name and email, e.g.
# username = Jane Doe <jdoe@example.com>
username =

[extensions]
# uncomment these lines to enable some popular extensions
# (see "hg help extensions" for more info)
#
# pager =
# progress =
# color =
~~~
{: .output}

Edit the file to set `username` to your own name and email address and add your
favourite editor in the `[ui]` section.
Also uncomment the `pager =`,
`progress =`,
and `color =` lines in the `[extensions]` section.
You can leave or delete the other comment lines (that start with `#`) as you wish.
When you are done your file should look something like:

~~~
[ui]
username = Doug Latornell <djl@douglatornell.ca>
editor = nano

[extensions]
pager =
progress =
color =
~~~
{: .output}

If you are on Windows,
please also add a `[color]` section to the file:

~~~
[color]
mode = win32
~~~
{: .output}

When you are finished,
save the file and exit your editor.

> ## hg: unknown command 'config'
>
> If your computer responds to the `hg config --edit` command with
>
> ~~~
> hg: unknown command 'config'
> ~~~
> {: .output}
>
> it means that you are using a version of Mercurial older than 3.0 that
> doesn't have the `hg config` command.
>
> You will have to create your configuration file from scratch.
> Windows users can use:
>
> ~~~
> nano $USERPROFILE/Mercurial.ini
> ~~~
> {: .bash}
>
> to create the file,
> and Mac OS X and Linux users can use:
>
> ~~~
> nano $HOME/.hgrc
> ~~~
> {: .bash}
>
> to create the appropriately named empty file and then type in the lines
> above.
{: .callout}

When you saved the file it was stored in your home directory as `.hgrc` on OS X and
Linux or as `Mercurial.ini` on Windows.
The fact that these settings are in the Mercurial configuration file in our home
directory means that they will be used for every Mercurial repository on this
machine.

The above configuration work only needs to be done the first time you use
Mercurial on a computer.

You can check that your settings are the way that you want them with the command

~~~
$ hg config ui extensions
~~~
{: .bash}

~~~
extensions.color=
extensions.progress=
extensions.pager=
ui.username=Doug Latornell <djl@douglatornell.ca>
~~~
{: .output}
