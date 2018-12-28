---
title: "Subversion: In-url revision browsing"
excerpt_separator: <!--more-->
---
I use [Subversion](http://subversion.tigris.org/) for revision control, and while most Svn tools (like [Subclipse](http://subclipse.tigris.org/), [TortoiseSVN](http://tortoisesvn.tigris.org/), and even the svn command-line client) offer decent log- and diff-tools, sometimes I find it easier to quickly pull up an Svn resource in a browser to look at it.
<!--more-->

Let's use the Google Code 'svn merge fairy' project for our guinea pig. Their [current trunk](http://svn-merge-fairy.googlecode.com/svn/trunk/) is at revision 8 at the time of writing.

Now, to easily browse an earlier revision of the tree, simply edit the URL and insert "```/!svn/bc/<revision number>/```" after the Svn root: [http://svn-merge-fairy.googlecode.com/svn/!svn/bc/1/trunk/]()

Here you can see that the first version (revision 1) of the project contained no files.
