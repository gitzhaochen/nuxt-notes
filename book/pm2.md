## pm2使用
> 使用ssh登录服务端部署nodejs程序，时间长了服务会挂掉极不稳定，使用pm2可以解决这个问题

使用pm2配置文件的方法，可以传相应的环境变量，默认是读取env环境下，ecosystem.config.js配置如下：
```sh 
module.exports = {
    apps : [
        {
            name: 'web',
            script: './server/index.js',
            env: {
                PORT: 3000,
                NODE_ENV: 'production'
            },
        }
    ]
}

``` 
然后npm start 即可开启服务，package.json配置如下：
```sh 
"scripts": {
    "dev": "cross-env NODE_ENV=development HOST=0.0.0.0 PORT=3000 nodemon server/index.js --watch server",
    "build": "nuxt build",
    "generate": "nuxt generate",
    "start": "pm2 start ecosystem.config.js"
},
```