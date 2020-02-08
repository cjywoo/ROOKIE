# 版本控制

## 语义化版本
语义化的版本遵循以下格式
```
1.2.3 - beta.1 + meta
主版本.次版本.修订号 - 先行版本 + 元数据(tag)
```

### 常见版本释义
alpha(内部测试版本) -> beta(公测版本) -> rc(发行候选版本) -> release

## 常见git平台
github, gitlab, gitea, gitee

### git工具的使用
* 版本回退
```
# 查看需要回滚的版本号
git reflog
# 回退到某个版本
git reset --hard 版本 
# 回退到上一个版本
git reset --hard head^
```

* 暂存缓存和回退
```
# 暂存
git stash
# 回退
git stash apply
```

* 代码提交问题
```
# 加入缓存
git add .
# 退出缓存
git reset 
```

### git Flow
使用于持续集成多环境版本
Master -> Pre-Production -> Production

分支管理的一些常用命令
```
# 查看远程分支的版本
git remote -v
# 从master分支上来去
git pull origin master
# 检查当前分支
git branch
# 删除dev分支
git branch -D dev
# 将远程分支删除
git push origin :分支
# 切换到一个新分支，并切换
git checkout -b dev
# 将某个分支合并到当前我的分支
git merge dev
```
> 注意，合并分支时，如果切换到master分支，然后用git merge dev命令，就是将dev分支合并到master分支上

### gitignore
* 去gitignore.io网站查看
* 一共.gitignore generator插件

如果文件先于gitignore生成，则需要删除快照才能生效
```
git rm --cached -r .
```
