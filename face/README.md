> 基于vue3 和 elementplus 开发的 **big组件**
>
> 生命绽放于战场,璀璨,却仅限于你的眼中

## 基础配置

> 组件地址：’http://zj.hblqyxjy.com/zj‘

## 初始模板

```js
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>模板代码</title>
        
        <link rel="stylesheet" href="http://zj.hblqyxjy.com/zj/static/css/el-com.css">
        <link rel="stylesheet" href="http://zj.hblqyxjy.com/zj/static/css/style.css">
        
        <link rel="stylesheet" href="//unpkg.com/element-plus/dist/index.css">
        <script src="//unpkg.com/vue@next"></script>
        <script src="//unpkg.com/element-plus"></script>
        <script src="//unpkg.com/@element-plus/icons-vue"></script>
        <script src="//unpkg.com/element-plus/dist/locale/zh-cn"></script>
        <script src="//unpkg.com/axios/dist/axios.min.js"></script>
    </head>
    <body>
        <div id="app" style="padding: 10px">
			// html标签
        </div>
        <script type="module">
            
            const App = {
                // 属性对象
                data() {
                    return {
                        
                    }
                },
                // 组件注册
                components: {},
                // 方法定义
                methods: {},
                // 生命周期 挂载后
                mounted(){}
            };
            const app = Vue.createApp(App);
            app.use(ElementPlus, {
                locale: ElementPlusLocaleZhCn,
            });
            for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
                app.component(key, component)
            }
            app.mount("#app");
        </script>
    </body>
</html>
```

> 组件使用
>
> 导入组件　=>　注册组件　=>　使用组件

```js
// 导入组件
import xxx from "../../widget/xxx.js";
// 注册组件
components: {
    xxx
},
// 使用组件
<xxx></xxx>
```