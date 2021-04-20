[toc]

# 本地分支管理

## 提交变更

```git
git commit -m "commit description"
```

## 撤销变更

本地分支：

`git reset` 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。

```
git reset HEAD~1
```

在reset后， `原HEAD`处 所做的变更还在，但是处于未加入暂存区状态。

远程分支：

```
git revert HEAD
```

## 撤销修改
```
git checkout -- <file>
```

## 删除文件
```
git rm <file>
```

## 创建分支

```git
git branch <分支名>
```

## 切换到分支

```
git checkout <分支名>
```

## 创建并切换到分支

```
git checkout -b <新分支名>
```

## 合并分支

```
git checkout <目标分支>;
git merge <被合并分支>
```

```
git checkout <被合并分支>;
git rebase <目标分支>
```

## 直接引用

```
git checkout <哈希值>
```

## 相对引用

在提交树上向上移动一步

```
git checkout <base>^
```

## 强制移动分支

````
git branch -f <分支名> <目标位置>
````

## 撤销变更

本地分支：

`git reset` 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。

```
git reset HEAD~1
```

在reset后， `原HEAD`处 所做的变更还在，但是处于未加入暂存区状态。

远程分支：

```
git revert HEAD
```

要撤销的提交记录后面会多了一个新提交！这是因为新提交记录  引入了**更改** —— 这些更改刚好是用来撤销 上一个 这个提交的。也就是说 新提交 的状态与 被撤销的状态的上一个是相同的。

## 复制提交

```
git cherry-pick <被复制的提交> ...
```

## 交互式 rebase

交互式 rebase 指的是使用带参数 `--interactive` 的 rebase 命令, 简写为 `-i`

如果你在命令后增加了这个选项, Git 会打开一个 UI 界面并列出将要被复制到目标分支的备选提交记录，它还会显示每个提交记录的哈希值和提交说明，提交说明有助于你理解这个提交进行了哪些更改。

```
git rebase -i <要修改的提交的父节点>
```

## 建立标签

```
git tag <tag-name> <指向记录>
```

# 远程仓库管理

## 克隆仓库

```
git clone
```

## 将本地仓库推到远程仓库

```
git remote add origin <地址>
git branch -M main
git push -u origin main
```


### git fetch

- 从远程仓库下载本地仓库中缺失的提交记录
- 更新远程分支指针(如 `o/main`)

`git fetch` 实际上将本地仓库中的远程分支更新成了远程仓库相应分支最新的状态。

`git fetch` 并不会改变你本地仓库的状态。它不会更新你的 `main` 分支，也不会修改你磁盘上的文件。

它可能已经将进行这一操作所需的所有数据都下载了下来，但是**并没有**修改你本地的文件

所以, 你可以将 `git fetch` 的理解为单纯的下载操作。


# 参考

[Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)