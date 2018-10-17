## jenkins构建生产环境
> nuxt build这步操作cpu占用很高，在服务器直接build时候，把服务器搞挂了，坑～,换方案：在jenkins构建完成后，只上传build后的文件到服务器，在服务器只跑start任务

package.json配置如下：
```sh 
"scripts": {
    "dev": "cross-env NODE_ENV=development HOST=0.0.0.0 PORT=3000 nodemon server/index.js --watch server",
    "build": "nuxt build",
    "generate": "nuxt generate",
    "start": "cross-env NODE_ENV=production node server/index.js"
},
```
方案步骤如下：
1. jenkins 拉代码 npm install（会自动更新package-lock.json，cnpm i 就不会）
2. npm run build 构建生产代码，在nuxt.config.js设置如下：
                       
```sh 
    /*
    ** Build configuration
    */
    buildDir: 'nuxt-dist',//build后生成的目录文件
    build: {
        /*
        ** You can extend webpack config here
        */
        extractCSS: true,//单独打包css文件
        plugins: [
            new webpack.ProvidePlugin({
                //设置全局变量
                '$': 'jquery',
                // ...etc.
            }),
        ],
        filenames: {
            //优化下build后的文件名称
            app: ({isDev}) => isDev ? '[name].js' : '[name].[chunkhash:8].js',
            chunk: ({isDev}) => isDev ? '[name].js' : '[name].[chunkhash:8].js',
            css: ({isDev}) => isDev ? '[name].js' : '[name].[contenthash:8].css',
            img: ({isDev}) => isDev ? '[path][name].[ext]' : 'img/[name]-[hash:8]-primas.[ext]',
            font: ({isDev}) => isDev ? '[path][name].[ext]' : 'fonts/[name]-[hash:8]-primas.[ext]',
            video: ({isDev}) => isDev ? '[path][name].[ext]' : 'videos/[name]-[hash:8]-primas.[ext]'
        },
        styleResources: {
            //对每个组件注入css变量
            stylus: ['./common/stylus/variable.styl', './common/stylus/mixin.styl'],
            options: {
                // See https://github.com/yenshih/style-resources-loader#options
                // Except `patterns` property
            }
        },
        extend(config, {isDev, isClient}) {
            if (isClient) {
                if (isDev) {
                    config.devtool = 'cheap-module-eval-source-map';
                }

            }
            config.module.rules.push(
                {
                    test: /tinymce[\/\\]content_style$/,
                    use: {
                        loader: 'raw-loader'
                    }
                }
            );
        },
    }
```
3. 上传静态文件到阿里云oss,静态文件在nuxt-dist/dist/client目录下,详情查看[静态文件cdn,部署到阿里云oss](./book/cdn.md)
4. 上传构建完成的文件到服务端,服务端运行只需要包括nuxt-dist/,server/,static/,config.js,ecosystem.config.js,nuxt.config.js,package.json,package-lock.json
5. 上传完成后在服务端npm install,安装项目依赖 express等
6. 运行npm start、服务开启
7. 但是时间长了服务会挂掉极不稳定，使用pm2可以解决这个问题，查看详情[服务端部署，使用pm2进程守护](./book/pm2.md)
