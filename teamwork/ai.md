# 自动化流程

## Eslint

```
# 安装eslint依赖
npm install -D eslint
# 初始化eslint
```

### 加载顺序的优先级
.eslintrc.js
.eslintrc.yaml
.eslintrc.yml
.eslintrc.json
.eslintrc
package.json

### 配置规则
* off(0):表示关闭
* warn(1): 表示警告
* error(2): 表示强制

### extends
在这个里面设置，既可以加载推荐的配置

### eslint配合vscode
* 安装eslint插件
* 全局安装eslint
* 点击右下角的eslint按钮

### 实现ctrl+s 自动修复
* command+shift+p,输入ui，然后再搜索里面输入auto,找到eslint，打开自动保存
* command+shift+p,输入json,设置settings

### settings自动同步
* 安装sync插件
* 使用command+shift+p 进行sync配置下载，同步即可
* 然后就能实现保存自动eslint了



## 前端自动化流程
前端自动化是指前端代码的自动化构建、打包、测试及部署等流程，通常与CI/CD流程相结合

自动化流程的意义：
* 减少人为失误，提高软件质量
* 效率迭代，便捷部署
* 快速交付，便于管理

### CI/CD 流程： 持续集成，持续部署

* jeckens
* circleCi
* travicCi

travicCi的配置方法可以见[链接](https://www.jianshu.com/p/ec5f57d96d66)