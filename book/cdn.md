## 静态文件cdn部署
> 静态文件部署到阿里云oss,加快页面打开速度，比如一些image、css和js文件

nuxt在build时候可以设置publicPath，在nuxt.config.js设置如下：
```sh 
    build: {
        publicPath: 'https://xxx-oss-cn-shanghai.aliyuncs.com/cdn/',
    }
```
build后的引用的静态资源都会加上这个前缀，所以我们需要在build后上传到目标文件下，这里使用阿里云ossutil上传工具，然后在jenkins里执行相关shell脚本即可，[阿里云ossutil文档参考](https://help.aliyun.com/document_detail/50452.html)