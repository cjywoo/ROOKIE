# node
这里介绍一些node的常用命令

## npx
利用npx指令，即使没有全局安装某个包，也可以使用指令，比如
```
npx webpack
# 等价于 node_modules/.bin/webpack
```

## npm-check-updates
全局安装该依赖，可以定期检查包是否有更新
```
# 安装依赖
npm install -g npm-check-updates
# 更新package.json
ncu -u
# 重新安装
npm install
```