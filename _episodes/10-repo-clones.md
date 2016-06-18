---
layout: page
title: Version Control with Mercurial
subtitle: Working with Clone Repositories
---
> ## Learning Objectives {.objectives}
>
> * Explain how to push, pull, update files, and update metadata among clones
>   of a repository.
> * Display a simple visualization of the state of a repository and explain
>   how updating the repository affects its state.

We're going to simulate working with a repository on different machines
(laptop and lab workstation,
home and work PCs,
local workstation and an HPC cluster,
etc.).
We'll do this by creating 2 clones of Susan's `forecast` repo from Bitbucket in different directories on our laptops.

Create 2 directories called `home` and `work` on your desktop:

~~~ {.bash}
$ cd ~/Desktop
$ mkdir home work
~~~

In the `work` directory,
clone the `forecast` repository from Bitbucket:

~~~ {.bash}
$ cd ~/Desktop/work
$ hg clone https://bitbucket.org/susan/forecast
~~~

`hg clone` creates a fresh local copy of a remote repository.

Now switch to the `home` directory and clone the `forecast` repo from Bitbucket
there too:

~~~ {.bash}
$ cd ~/Desktop/home
$ hg clone https://bitbucket.org/susan/forecast
~~~

Our computer now has two new copies of the repository:

![After Creating `work` and `home` Clones of Repository](fig/hg-after-home-work-clones.svg)

Let's change the `work` copy of the repository by adding a new file in which we're going to start building the bibliography for the paper that will eventually be written:

~~~ {.bash}
$ cd ~/Desktop/work/
$ nano biblio.txt
~~~

and type into that file:

~~~ {.output}
NEMO Ocean Engine, Version 3.4. Madec G., et al. 2012.
~~~

Tell Mercurial to track our new file,
and commit it:

~~~{.bash}
$ hg add biblio.txt
$ hg commit -m"Start creating bibliography for the forecast paper."
~~~

then push the change to Bitbucket:

~~~ {.bash}
$ hg push
~~~
~~~ {.output}
pushing to https://bitbucket.org/susan/forecast
searching for changes
http authorization required for https://bitbucket.org/susan/forecast
realm: Bitbucket.org HTTP
user: susan
password:
adding changesets
adding manifests
adding file changes
added 1 changesets with 1 changes to 1 files
~~~

Note that we didn't have to create a remote path called `default`;
Mercurial did that automatically when we cloned the repository from Bitbucket.

Our three repositories now look like this:

![After Pushing Change from `work` Repository](fig/hg-after-change-to-work-clone.svg)

We can now download the change into our `home` repository:

~~~ {.bash}
$ cd ~/Desktop/home/
$ hg pull
~~~
~~~ {.output}
pulling from https://bitbucket.org/susan/forecast
searching for changes
http authorization required for https://bitbucket.org/susan/forecast
realm: Bitbucket.org HTTP
user: susan
password:
adding changesets
adding manifests
adding file changes
added 1 changesets with 1 changes to 1 files
(run 'hg update' to get a working copy)
~~~
If we look at our repository history now with the `hg log --graph` command we can see that the `@` character marks the revision that our working copy of the files is at and the revision that we pulled from Bitbucket is above that,
meaning it has not yet been applied to our working files:

~~~ {.bash}
$ hg log --graph
~~~
~~~ {.output}
o  changeset:   3:68fd235a0541
|  user:        Susan Allen <sallen@eos.ubc.ca>
|  date:        Sun Jun 14 11:04:44 2015 -0700
|  summary:     Start creating bibliography for the forecast paper.
|
@  changeset:   2:d14b5f2372fc
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

To apply those changes we use `hg update`
(as Mercurial helpfully suggested at the end of the `hg pull` process):

~~~ {.bash}
$ hg update
~~~
~~~ {.output}
1 files updated, 0 files merged, 0 files removed, 0 files unresolved
~~~

You can use `hg log --graph` again to see that the `@` has been moved to changeset 3 which tells us that our working copy is up to date.

Here is what our two local repositories and our Bitbucket repository look like now,
showing how the `biblio.txt` file has been copied from Bitbucket to our `home` repository clone:

![After Pulling Change to 'home' Repository](fig/hg-after-pulling-to-home-clone.svg)

Generally,
we are unlikely to have multiple copies of the same remote repository on our laptop at once.
Instead,
one of those copies would be on our laptop,
and the other on a lab workstation (for example).
Pushing and pulling changes gives us a reliable way to share work between different machines,
and,
as we will see shortly,
different people.
