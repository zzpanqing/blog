
# Git #

# rebase squash 将多个 commits 合并成一个 #
[link](http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html)
将最后四个 commits 合并

    $ git rebase -i HEAD~4
* git 打开 editor 将 除第一行外的其他行开头的 commit 都改成 squash。

* 保存后，另外一个 editor 被打开，让你改写 commit message

#资源#
[Git Ready](http://gitready.com/)
