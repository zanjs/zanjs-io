---
title: Git的10种撤销操作详解
date: 2016-07-21 09:28:46
tags:
  - git

categories:
  - git

---

## 撤销一个公共修改

> Undo a “public” change

场景：你刚刚用 `git push` 将本地修改推送到了`GitHub`，这时你意识到在提交中有一个错误。

你想撤销这次提交。使用撤销命令：`git revert`

发生了什么：`git revert` 将根据给定SHA的相反值，创建一个新的提交。

如果旧提交是 `matter`，那么新的提交就是 `anti-matter` ——旧提交中所有已移除的东西将会被添加进到新提交中，
旧提交中增加的东西将在新提交中移除。

这是 `Git` 最安全、也是最简单的“撤销”场景，因为这样不会修改历史记录——你现在可以 `git push `下刚刚 `revert` 之后的提交来纠正错误了。


## 修改最近一次的提交信息

> Fix the last commit message

场景：你只是在最后的提交信息中敲错了字，
比如你敲了 `git commit -m “Fxies bug #42″`，
而在执行 `git push` 之前你已经意识到你应该敲 `”Fixes bug #42″`。

使用撤销命令：`git commit –amend` 或 `git commit –amend -m “Fixes bug #42″` ;

发生了什么：`git commit –amend` 将使用一个包含了刚刚错误提交所有变更的新提交，来更新并替换这个错误提交。
由于没有 `staged` 的提交，所以实际上这个提交只是重写了先前的提交信息。


## 撤销本地更改

> Undo “local” changes

场景：你正在编辑的文件被保存了但是你的编辑器恰在此时崩溃了。
此时你并没有提交过代码。你期望撤销这个文件中的所有修改——将这个文件回退到上次提交的状态。

使用撤销命令：`git checkout —`

发生了什么：`git checkout` 将工作目录 `（working directory）` 里的文件修改成先前 `Git` 已知的状态。
你可以提供一个期待回退分支的名字或者一个确切的`SHA码` ，`Git` 也会默认检出 HEAD——即：当前分支的上一次提交。

注意：用这种方法“撤销”的修改都将真正的消失。它们永远不会被提交。
因此Git不能恢复它们。此时，一定要明确自己在做什么！（或许可以用 `git diff` 来确定）


## 重置本地修改

>  Reset “local” changes

场景：你已经在本地做了一些提交（还没push），
但所有的东西都糟糕透了，你想撤销最近的三次提交——就像它们从没发生过一样。


使用撤销命令：`git reset` 或 `git reset –hard`


发生了什么：`git reset` 将你的仓库纪录一直回退到指定的最后一个SHA代表的提交，
那些提交就像从未发生过一样。

默认情况下，`git reset` 会保留工作目录`（working directory）`。
这些提交虽然消失了，但是内容还在磁盘上。
这是最安全的做法，但通常情况是：你想使用一个命令来“撤销”所有提交和本地修改——那么请使用 `–hard` 参数吧。

## 撤销本地后重做

> Redo after undo “local”

场景：你已经提交了一些内容，并使用 `git reset –hard` 撤销了这些更改（见上面），
突然意识到：你想还原这些修改！

使用撤销命令：`git reflog` 和 `git reset` , 或者 `git checkout`

发生了什么：`git reflog` 是一个用来恢复项目历史记录的好办法。你可以通过 `git reflog` 恢复几乎任何已提交的内容。

你或许对 `git log` 命令比较熟悉，它能显示提交列表。
`git reflog` 与之类似，只不过 `git reflog` 显示的是HEAD变更次数的列表。
一些说明：

1. 只有HEAD会改变。当你切换分支时，用 `git commit` 提交变更时，或是用 `git reset` 撤销提交时，HEAD都会改变。
但当你用 `git checkout` –时， HEAD不会发生改变。（就像上文提到的情形，那些更改根本就没有提交，因此 `reflog` 就不能帮助我们进行恢复了）

2.  `git reflog` 不会永远存在。Git将会定期清理那些“不可达（unreachable）”的对象。不要期望能够在reflog里找到数月前的提交记录。

3. `reflog` 只是你个人的。你不能用你的 `reflog` 来恢复其他开发者未 `push` 的提交。
因此，怎样合理使用 `reflog` 来找回之前“未完成”的提交呢？这要看你究竟要做什么：

    - 如果你想恢复项目历史到某次提交，那请使用 `git reset –hard`

    - 如果你想在工作目录（working direcotry）中恢复某次提交中的一个或多个文件，并且不改变提交历史，那请使用 `git checkout–`

    - 如果你想确切的回滚到某次提交，那么请使用 `git reset`