##Markdown抄笔记之vue学习之路
>本文是一份vue.js学习笔记，用以本人学习vue.js。内容涵盖vue.js 的基本知识，文章顺序基本按照官方文档的顺序，每个知识点都有附上代码，然后根据个人理解给予一些注解。

####1.vue.js是什么
Vue.js （读音/vju:/,类似于view）是一套构建用户界面的**渐进式框架**。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，它不仅容易上手，还便于与第三方库或既有项目整合。另一方面，当与单文件组件和vue生态系统支持的库结合使用时，Vue 也完全能够为复杂的单页应用程序提供驱动。

####2.Vue的基本用法
首先下载vue.js引入文件中或者使用cdn将
`<script src="https://unpkg.com/vue/dist/vue.js"></script>`
引入html文件中，接下来就可以进行vue.js的入门编写

######2.1  开始认知vue.js
>开始创建一个根据构造函数Vue的一个Vue根实例\


html：
````
<div id='demo'></div>
````
---
javascript：
````
var vm = new Vue({
	el:'#demo',
    data:{},
    methods:{}
})
````

* 使用Vue 构造函数创建Vue实例，然后通过Vue实例的el接口'#id'实现和HTML元素的挂载；
* 构造函数Vue需要传入一个选项对象，可包含挂载元素（el:'#id'）、数据(data:{})、方法(methods:{})和生命周期钩子等；

######2.2 Vue实例的属性和方法
>Vue实例将代理data对象的所有属性，也就是说部署在data对象上的所有属性和方法豆浆直接成为Vue的属性和方法

html:
````
	<div id='demo'>
    	{{message}}
    </div>
````

javascript:
````
	var demo = new Vue({
    	el:'#demo',
        data:{
        	message:'hello vue!'
        }
    })
    //vue.js提供$进行获取demo这一实例的对象
    demo.$el === documenr.getElementById('demo')//true
    app.$data.message//hello vue
````

vue是具有响应式更新的，当在控制台改变demo.message的值时，前台视图会实时更新，但是只有在实例创建的同时进行初始化才具有响应式更新，若在实例创建之后添加是不会触发视图更新的。

######2.3数据绑定
>绑定文本和HTML

html:
````
	<div id='app1'>
    	{{message}}
        <div v-html='hi'></div>
    </div>
````
javascript:
````
	var app1 = new Vue({
    	el:'#app1',
        data: {
        	message: 'hello vue！'，
            hi: '<h1>hi</h1>'
        }
    })
````

* HTML部分实现数据的动态绑定，这个数据是vue实例的属性值
* JS部分的语法可以从jquery角度去理解，相当于创建一个Vue实例，这个实例指向#app1，并在Vue提供的固定接口data上定义Vue实例的属性；
* 使用{{message}}的mustache语法只能将数据解释为纯文本，为了输出HTML可以使用v-html指令；

>绑定数据在元素属性上

html:
````
	<div id='app' v-bind:title='message' v-bind='red' v-once>
		{{message}}
    </div>
````
javascript:
````
	var app = new Vue({
    	el:'#app',
        data:{
        	message:'hello vue!',
            red:'color:red'
        }
    })
````

* 定义在Vue实例的data接口上的数据绑定是灵活的，可以绑定在DOM节点内部，也可以绑在属性上；
* 绑定数据袋节点属性上时，需要使用v-bind指令，这个元素节点的title属性和Vue实例的message属性绑定到一起，从而建立数据与该属性值的绑定，也可以使用v-bind:href='url'的缩写方式:href='url';
* v-once指令能够让你执行一次性的插值，当数据改变时，插值处的内容不会更新;

>使用JS表达式处理数据

html:
````
	<div id='app'>
		<p v-once>{{num + 10}}</p>
    	<p v-if='seen'>{{message + 'vue'}}</p>
	</div>
````
Javascript:
````
	var app = new Vue({
    	el:'#app',
        data:{
        	num:10,
            message: 'hello vue',
            seen:true
        }
    })
````

* v-once 只一次有效，随后更改app.num 视图不改变
* v-if 当布尔值值为true时DOM存在，当布尔值为false时DOM不存在

>使用过滤器来格式化数据

html:
````
	<div id='app'>
    	<p v-if="seen">{{message | capitalize}}</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
            message: 'hello vue!',
            seen: true
        },
        filters: {
            capitalize:function (value) {
                if (!value) return ''
                value = value.toString()
                return value.charAt(0).toUpperCase() + value.slice(1)
            }
        }
    })
````

>条件指令控制DOM元素的显示操作

html：
````
	<div id='app'>
    	<p v-if="seen">{{message}}</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
            message:'hello vue!',
            seen: true
        }
    })
````

>循环指令实现数据的遍历

html：
````
	<div id='app'>
    	<ol>
        	<li v-for="item in items">
            	{{ item.text }}
        	</li>
    	</ol>
	</div>
````
Javascript:
````
	    var app = new Vue({
        el:'#app',
        data:{
            items:[
                {text:'Vue'},
                {text:'Angular'},
                {text:'React'}
            ]
        }
    })
````

* v-for可以绑定数组型数据进行绑定，并使用item in items形式进行数据的遍历操作

> 事件绑定指令可以实现事件监听

html:
````
	<div id='app'>
    	<p>{{message}}</p>
    	<button v-	on:click="reverseMessage">reverseMessage</button>
	</div>
````
Javascript:
````
	   var app = new Vue({
        el:'#app',
        data:{
            message:'hello vue!'
        },
        methods:{
            reverseMessage:function () {
                this.message = this.message.split('').reverse().join('');
            }
        }
    })
````













































































































































































