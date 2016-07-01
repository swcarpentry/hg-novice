---
title: Exploring History
teaching: 10
exercises: 5
questions:
- "How do I review my changes?"
- "How can I recover old versions of files?"
objectives:
- "Compare files with older versions of themselves."
- "Display the changes that were made to files in a previous changeset."
keypoints:
- "Use `hg diff` to compare different versions of files."
---

If we want to see what we changed when,
we use `hg diff` again,
but refer to old versions using the `--rev` or `-r` flag and the revision numbers:

~~~
$ hg diff --rev 1:2 plan.txt
~~~
{: .bash}

~~~
diff -r b31241913818 -r 2e15a7ee29c2 plan.txt
--- a/plan.txt  Tue Jun 09 15:16:11 2015 +0200
+++ b/plan.txt  Tue Jun 09 15:28:25 2015 +0200
@@ -1,3 +1,4 @@
 Goal: Run NEMO everyday to forecast storm surge water levels

 Need daily high resolution weather forcing from EC.
+Also need daily average Fraser River flow from EC.
~~~
{: .output}

~~~
$ hg diff -r 0:2 plan.txt
~~~
{: .bash}

~~~
diff -r 1320339bbcae -r 2e15a7ee29c2 plan.txt
--- a/plan.txt  Tue Jun 09 14:41:27 2015 +0200
+++ b/plan.txt  Tue Jun 09 15:28:25 2015 +0200
@@ -1,1 +1,4 @@
 Goal: Run NEMO everyday to forecast storm surge water levels
+
+Need daily high resolution weather forcing from EC.
+Also need daily average Fraser River flow from EC.
~~~
{: .output}

In this way,
we build up a chain of revisions.
The most recent end of the chain is the changeset with the highest revision number.

To see what changes were made between a particular changeset and its parent
use the `--change` or `-c` flag:

~~~
$ hg diff --change 1
~~~
{: .bash}

~~~
diff -r 1320339bbcae -r b31241913818 plan.txt
--- a/plan.txt  Tue Jun 09 14:41:27 2015 +0200
+++ b/plan.txt  Tue Jun 09 15:16:11 2015 +0200
@@ -1,1 +1,3 @@
 Goal: Run NEMO everyday to forecast storm surge water levels
+
+Need daily high resolution weather forcing from EC.
~~~
{: .output}
