## Axios

本地使用反向代理解决跨域问题，nuxt.config.js配置如下

```sh
    //Axios module configuration
    modules: [
            // Doc: https://github.com/nuxt-community/axios-module#usage
            '@nuxtjs/axios',
        ],
    axios: {
        // See https://github.com/nuxt-community/axios-module#options
        baseUrl: '/',
        credentials: true,
        proxy: true
    },
    proxy: {
        '/server_url': {
            target: 'https://xxx',
            pathRewrite: {"^/server_url": "/v2"},
            changeOrigin: true,
            secure: false
        }
    },
```

在vue文件中，使用aysncData获取数据

```sh
    async asyncData ({app, params, error }) {
        try{
            let url = '/server_url/' + params.id ;
            let data= await app.$axios.$get(url);
            // app.$axios.$get(url) 等价于 app.$axios.get(url).data
            return { original: data.data }
        }catch(err){
            error({ statusCode: 404, message: 'Page not found' });
        }
    }
```