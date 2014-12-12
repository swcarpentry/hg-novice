---
layout: page
title: Version Control with Mercurial
subtitle: Reference
---
## A Better Kind of Backup

*   Create a `~/.hgrc`
    (or `%USERPROFILE%\Mercurial.ini` on Windows) to configure a user name,
    email address, editor, and other preferences once per machine.
*   `hg init` initializes a repository.
*   `hg status` shows the status of a repository.
*   Files are stored in a project's working directory (which users see),
    and the local repository (where committed snapshots are permanently recorded).
*   `hg add` tells Mercurial to track files.
*   `hg commit` creates a snapshot of the changes to 1 or more files in the local repository.
*   Always write a log message when committing changes.
*   `hg diff` displays differences between revisions.
*   `hg revert` recovers old versions of files.
*   The `.hgignore` file tells Mercurial what files to ignore.

## Collaborating

*   A local Mercurial repository can be connected to one or more remote repositories.
*   Use the HTTPS protocol to connect to remote repositories
    until you have learned how to set up SSH.
*   `hg push` copies changes from a local repository to a remote repository.
*   `hg pull` copies changes from a remote repository to a local repository.
*   `hg clone` copies a remote repository to create a local repository
    with a remote called `default` automatically set up.

## Conflicts

*   Conflicts occur when two or more people change the same file(s) at
    the same time.
*   The version control system does not allow people to blindly
    overwrite each other's changes. Instead, it highlights conflicts
    so that they can be resolved.

## Open Science

*   Open scientific work is more useful and more highly cited than closed.
*   People who incorporate GPL'd software into theirs must make theirs open;
    most other open licenses do not require this.
*   The Creative Commons family of licenses allow people to mix and match
    requirements and restrictions on attribution,
    creation of derivative works,
    further sharing,
    and commercialization.
*   People who are not lawyers should not try to write licenses from scratch.
*   Projects can be hosted on university servers,
    on personal domains,
    or on public forges.
*   Rules regarding intellectual property and storage of sensitive information apply
    no matter where code and data are hosted.

## Glossary

changeset
:   FIXME

clone
:   FIXME

conflicts
:   FIXME

infective
:   FIXME

Internet Service Provider (ISP)
:   FIXME

merge
:   FIXME

protocol
:   FIXME

remote
:   FIXME

repository
:   FIXME

resolve
:   FIXME

timestamp
:   FIXME
