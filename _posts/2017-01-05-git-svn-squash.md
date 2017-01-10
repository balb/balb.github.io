---
layout: post
title:  "git-svn squash"
---

At work we still use Subversion but I like to work locally with git then push my changes back to Subversion. 

If I push a bunch of local commits back to Subversion then multiple builds are kicked off on the continuous integration server. This isn't great.

To prevent this I now squash the pending commits into 1 before pushing.

[This is a nice guide to squashing](http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html)