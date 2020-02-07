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
```

* 暂存缓存和回退
```
# 暂存
git stash
# 回退
git stash apply
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
# 切换到一个新分支，并切换
git checkout -b dev
```
