#### 显示/隐藏

- v-show 如果为 false 时通过 css 样式将元素隐藏 (display:none/block)

```html
    <div v-show='false'></div>
```

- v-if 如果为 false 时移除 DOM 节点(removeChild)

```html
    <div v-if='false'></div>
```

---

#### computed vs methods

- 我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。可以说使用 computed 性能会更好，但是如果你不希望缓存，你可以使用 methods 属性。

---

#### 实例

- 使用 Object.freeze()，这会阻止修改现有的属性，也意味着响应系统无法再追踪变化。除了数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。

---

####

不要在选项属性或回调上使用箭头函数，比如 `created: () => console.log(this.a)`或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误。

通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：
这也同样意味着下面的计算属性将不再更新，因为 `Date.now() `不是响应式依赖：
``` vue
computed: { 
    now: function () { 
        return Date.now() 
    }
}
```
我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

如果你不希望有缓存，请用方法来替代。

#### vue 1.0跟2.0的一些区别
- 绑定一次
``` vue
    {{*msg}}
    <div v-once>{{msg}}</div>
```
vue2.0已废弃 请使用v-once
- 绑定html代码
``` vue
    {{{msg}}}
    <div v-html="msg"></div>
```
{{{msg}}}写法vue2.0已废弃,请使用v-html
- 循环v-for数组
    1.0默认通过value进行遍历(key,value),遍历需加track-by="$index"(不加重复数据不绑定)
    2.0通过key进行遍历(value,key)

    ``` vue
    data:{
        arr:['苹果','橘子','香蕉']
    }
    <ul>
        <li v-for="value in arr">
            {{value}} {{$index}}
        </li>
    </ul>
    ```
{{{$index}}}写法vue2.0已废弃
- 对象
``` vue
    data:{
        json:{name:'zfpx'}
    }
    <li v-for="value in json" >
        {{value}}
    </li>
```
{{$key}}和{{$index}}vue2.0已废弃

- 对象数组都可以进行解构赋值
  v-for = '(val,index) in arr'
// 2.0 必须进行定义，不然会报错

- 事件v-on
``` html
<button v-on:click="addFruit($event)">按钮</button>
<ul>
    <li v-for="value in json" >
       {{value}}
    </li>
</ul>
var vue = new Vue({
    el:'#box',
    data:{
        json:['香蕉','苹果','橘子']
    },
    methods:{
        addFruit(event){
            this.json.push('苹果');
        }
    }
});
```
- v-on:click简写@click 2.0支持
执行方法时加上()事件源默认不传递，需要手动传入$event
methods中的this永远指向Vue的实例


