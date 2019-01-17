
# weex_vue语法规则.md

## 资源
- [vue.js首页][1]  
- [在 Weex 中使用 Vue.js][2]  
- [vue教程v2.x][3]
- [vue - guide][5]
- [API - Vue 2.x][4]

## 结构

```html
<template>
    <div style="justify-content:center">
        <text class="freestyle">{{greet}}</text>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                greet: "fetching..."
            }
        },
        create () {
            this.greet = "Hello Weex"
        }
    }
</script>

<style scoped>
    .freestyle {
        color: #41B883;
        font-size: 233px;
        text-align: center;
    }
</style>
```

1. 标签在上，js在中，样式style在下；  

2. weex_vue源代码文件的后缀名为`.vue`  

3. 一个vue文件表示一个组件。  


## 标签

1. 标签部分根标签为`<template></template>`。template可能是模板的意思  

2. 标签中的样式，可以在标签上直接指定，也可以使用class指定，但不能用标签作为选择器  

3. `<text>`是weex特有标签，是一个块级文本容器，用来渲染文字。文本只能放在`<text>`标签里，否则将被忽略。  

4. 只能使用weex内置组件，或者基于内置组件通过js封装的上层组件。  


## javascript

1. vue页面有自己的数据和生命周期。export返回的default对象包含页面的数据和生命周期方法。default对象的`data()`方法返回的对象就是页面的数据，返回对象中的数据可以在标签中通过双大括号引用，形如`{{count}}`。default对象的`create()`方法在页面创建时回调，方法中的`this`表示`data()`返回的对象。  


## style

1. 样式根标签为`<style scoped></style>`，注意`scoped`不能少。`scoped`表示css样式只能作用于当前组件。  


[1]: https://vuejs.org/
[2]: https://weex.incubator.apache.org/cn/guide/use-vue.html
[3]: https://cn.vuejs.org/v2/guide/
[4]: https://cn.vuejs.org/v2/api/
[5]: https://vuejs.org/v2/guide/