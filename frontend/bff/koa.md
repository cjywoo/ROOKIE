# koa
一个全新的web框架，致力于成为web应用和api开发领域中的一个更小、更富表现力、更健壮的基石

利用async函数丢弃回调函数，并增强错误处理。Koa没有任何预置的中间件，可快速愉快地编写服务端应用程序

## 核心概念
* Koa application(应用程序)
* Contex(上下文)
* Request(请求)，Response

## 一个最基础的koa应用
```
const Koa = require('koa');
const app = new Koa();
app.use(async(ctx)=>{
    ctx.body ='hello woo';
});

app.listen(3000);
console.log('app is starting in port 3000');
```

## 一个基础的中间件写法
```
const Koa = require('koa');
const app = new Koa()

const middleware = function async(ctx, next) {
    console.log('this is a middleware')
    console.log(ctx.request.path)
    next()
}

app.use(middleware)

app.listen(3000)
```

## koa 开发restful接口
利用koa-router和koa-body进行开发

```
const Koa = require('koa');
const Router = require('koa-router')
const cors = require('@koa/cors')
const koaBody = require('koa-body')

const app = new Koa();
const router = new Router()

router.prefix('/api')

// 1. request, method, respond
// 2. api url => function, router
router.get('/', ctx => {
    console.log(ctx)
    ctx.body ='hello woo';
})

router.get('/api', ctx => {
    const params = ctx.request.query
    console.log(params)
    console.log(ctx)
    ctx.body ='hexwllo api';
})

router.post('/post', async (ctx) => {
    let {body} = ctx.request
    console.log(body)
    console.log(ctx.request)
    ctx.body = {
        ...body
    }
})

app.use(koaBody())
app.use(cors())

// 3. ctx, async
app.use(router.routes())
    .use(router.allowedMethods())

app.listen(3000);
console.log('app is starting in port 3000');

```

## 设计一个合理的开发目录结构
* 按照功能模块进行区分
* 路由压缩： koa-combile-routers
* 静态资源： koa-static
* 安全头： koa-helmet

## koa热加载
* 利用Nodemon包来实现热加载
安装nodemon之后，使用`npx nodemon src/index.js即可`
* 使用es6语法启动koa
改写成es6之后，用`npx babel nodemon src/index.js`即可

## koa配置优化
* 利用koa-compose进行中间件的整合
```
const middleware = compose([
    koaBody(),
    statics(path.join(__dirname,'../static')),
    cors(),
    jsonutil({pretty:false, param:'pretty'}),
    helmet()
])
app.use(middleware)
```
* 利用webpack-merge进行配置拆分
* 利用terser-webpack-plugin进行代码压缩
* 利用SplitChunksPlugin避免重复依赖


