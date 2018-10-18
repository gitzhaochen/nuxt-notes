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

在vue文件中，使用aysncData获取数据填充data，区别fetch 方法用于在渲染页面前填充应用的状态树（store）数据， 与 asyncData 方法类似，不同的是它不会设置组件的数据。

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
    },
    async fetch({app, store, params}) {
        let {data} = await app.$axios.$get('/api')
        store.commit('updateAppVersion', data)
    },
```