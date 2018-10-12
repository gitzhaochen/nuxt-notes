## element-ui
element主题换色及国际化，配置如下

nuxt.config.js配置全局样式，使用element命令行主题工具生成theme，
[命令行主题工具相关地址](http://element-cn.eleme.io/#/zh-CN/component/custom-theme),
（在项目中改变 SCSS 变量这种方法我试了没有成功，目前没有找到原因）
```sh 
    css: [
            '~/theme/index.css'
        ],

    router: {
        middleware: 'i18n'
    },
    /*
    ** Plugins to load before mounting the App
    */
    plugins: [
        '~/plugins/element-ui',
        '~/plugins/i18n',
   
    ],
```
在plugins中的element-ui.js代码如下：
```sh 
    import Vue from 'vue'
    import Element from 'element-ui'
    
    export default ({ app }) => {
        Vue.use(Element, { i18n: (key, value) => app.i18n.t(key, value) })
    }

```

然后在vue文件中语言下拉如下
```sh 
    <template>
        <el-select class="language" v-model="lang" @change="langChange(lang)"
                   placeholder="english" size="mini">
            <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
            </el-option>
        </el-select>
    </template>

    <script type="text/ecmascript-6">
    
        export default {
            name: '',
            components: {},
            data() {
                return {
                    lang: this.$i18n.locale,
                    options: [
                        {
                            value: 'en',
                            label: 'English'
                        },
                        {
                            value: 'zh',
                            label: '中文'
                        },
                        {
                            value: 'ja',
                            label: '日本語'
                        },
                        {
                            value: 'ko',
                            label: '한국어'
                        }
                    ]
                }
            },
            methods:{
                langChange(val) {
                    this.$store.commit('SET_LANG', val);
                    this.$i18n.locale  = this.$store.state.locale
                }
            }
        }
    </script>
```