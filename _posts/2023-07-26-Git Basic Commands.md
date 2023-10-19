---
layout: post
author: Yasu
tags: [Git, note to self]
---
While I was struggling to create this blog, I got familir with 4 basic Git commands below.

1. git add . : Put all the modified files in the current folder on the stage, meaning that get those files ready to be commited.  
2. git status : See all the files name which were added by the "git add ." command.
3. git commit -m "Commit Message" : Save on the current branch the changes added by the "git add ." command.
4. git push : Upload the changes to a remote repository.

P.S.
If you want to see how your post looks, you can test it on your localhost by executing this command:

**bundle exec jekyll serve**