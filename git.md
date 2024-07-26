## 第一次提交


## 二次提交
```
确保分支
git branch

更新当前代码，拉取仓库新代码
git pull

添加修改代码到缓冲区
git add .

提交到本地
git commit -m "提交信息"

推送
git push

如果失败就说明没有设置默认推送分支
git push --set-upstream origin master
```

```
修改已提交的提交信息：
如果你只是想修改最近一次提交的提交信息，可以使用git commit --amend命令。这会打开文本编辑器，让你修改提交信息。完成修改后，保存并关闭编辑器，然后使用git push --force（或git push --force-with-lease以避免丢失他人的提交）将更改推送到远程仓库。
回退并提交新的更改：
如果需要回退到某个旧的提交，并在此基础上添加新的更改，可以使用git reset命令（如git reset --soft HEAD~1来回退到上一个提交，并保留工作区的更改）。然后，按照上述步骤添加和提交新的更改。
注意事项
在使用git push --force或git reset等命令时，请务必谨慎，因为它们会改变项目的历史记录，可能导致与他人的协作出现问题。
如果你的代码已经被其他人克隆或拉取，修改历史记录可能会导致他们遇到冲突。在这种情况下，最好先与团队成员协商。
确保你的提交信息清晰、准确，并遵循项目或团队的提交规范。
通过以上步骤，你可以成功地在Gitee上再次提交更改到已经存在的仓库中。
```

```
git push gitee master   # 同样假设你的分支是master

git push githup master   # 同样假设你的分支是master
```