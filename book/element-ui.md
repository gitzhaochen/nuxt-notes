## element-ui
element主题换色及国际化，配置如下

nuxt.config.js配置全局样式，改变 SCSS 变量自定义主题颜色
```sh 
    css: [
            '~/common/element-ui.scss'
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
在element-ui.scss中$--font-path变量使用相对路径:
```sh
    /* 改变主题色变量 */
    $--color-primary: #ed5634;
    
    /* 改变 icon 字体路径变量，必需 */
    $--font-path: './../../node_modules/element-ui/lib/theme-chalk/fonts';
    
    @import "~element-ui/packages/theme-chalk/src/index";

```
plugins中的element-ui.js代码如下：
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