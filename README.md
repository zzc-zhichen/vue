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

* v-on指令用于监听事件操作，click="reverseMessage"定义点击事件后执行的回调函数;
* v-on指令也可以采用缩写方式: @click="method"
* 在Vue实例中，提供methods接口用于统一定义函数

>结语

章节涉及Vue的基础数据绑定操作，内容主要为：

 * {{message}}实现文本数据的绑定;并且文本数据可以使用JS表达式和过滤器进行进一步处理;
 * v-html="hi"HTML数据的绑定
 * v-bind:href="url"实现属性数据的绑定;
 * v-if="seen"和v-for="item in items"指令实现流程控制
 * v-on:click="method"指令实现时间监听

2.4计算属性
>使用计算属性完成一些数据计算操作

html:
````
	<div id='app'>
    	<p>Original message : {{message}}</p>
    	<p>Reversed message : {{ReversedMessage}}</p>
	</div>
````
Javascript:
````
	    var app = new Vue({
        el:'#app',
        data:{
            message:'hello vue!'
        },
        computed:{
            ReversedMessage:function () {
               return this.message.split('').reverse().join('');
            }
        }
    })
````

* Vue实例提供computed对象,我们可以在对象内部定义需要进行计算的属性ReverseMessage,而提供的函数将作为属性的getter,即获取器
* 上述的代码使得app.ReverseMessage依赖于app.message;
* 与之前直接在{{message.split('').reverse().join('')}}使用表达式相比，它让模板过重并且难以维护代码

>计算属性 VS Methods

html:
````
	<div id='app'>
    	<p>Original message : {{message}}</p>
    	<p>Reversed message : {{ReversedMessage}}</p>
    	<p>Reversed message : {{reversedMessage()}}</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
            message:'hello vue!'
        },
        computed:{
            ReversedMessage:function () {
               return this.message.split('').reverse().join('');
            }
        },
        methods:{
            reversedMessage:function () {
                return this.message.split('').reverse().join('')
            }
        }
    })
````

* 通过Vue实例的methods接口,我们在模板中调用reversedMessage函数同样实现需求;
* methods与computed方法的区别在于：computed的数据依赖于app.message,只要message未变,则访问ReverseMessage计算属性将立即返回之前的计算结果,而methods则每次重新渲染时总是执行函数;
* 如果有缓存需要,请使用computed方法,否则使用methods替代;

>计算属性的setter

Vue实例的computed对象默认属性只有getter,如果你要设置数据,可以提供一个setter,即设置器;

html：
````
	<div id='app'>
    	<p>Hi,I'm{{fullName}}</p>
	</div>
````
Javascript：
````
    var app = new Vue({
        el:'#app',
        data:{
            message:'hello vue!',
            name:'Tom'
        },
        computed:{
            fullName:{
                get:function () {
                    return this.name
                },
                set:function (value) {
                    this.name = value
                }
            }
        }
    })
````

2.5Class与Style的绑定
>绑定Class

Html:
````
	<div id='app'>
    	<!--直接绑定对象的内容-->
    	<p class="static" v-bind:class="{active:isActive,error:hasError}">Hello world</p>
    	 <!--绑定对象-->
    	<p v-bind:class="classObj">啊偶</p>
    	<p v-bind:class="style">你好</p>
    	<!--绑定数组-->
    	<p v-bind:class="[staticClass,activeClass,errorClass]">奥布拉</p>
    	<button @click="changeColor">click me</button>
	</div>
````
Css:
````
    .static{
    	width: 200px;
        height: 100px;
        background: #cccccc;
    }
    .active{
        color: red;
    }
    .error {
        font-weight: 800;
    }
````
Javascript:
````
   var app = new Vue({
        el:'#app',
        data:{
            isActive:true,
            hasError:true,
            classObj:{
                static:true,
                active:true,
                error:true
            },
            staticClass:'static',
            activeClass:'active',
            errorClass:'error'
        },
        computed:{
            style:function () {
                return{
                    active:this.isActive,
                    static:true,
                    error:this.hasError
                }
            }
        },
        methods: {
            changeColor:function () {
                this.isActive = !this.isActive
            }
        }
    })
````

* 通过v-bind:class="{}"或v-bind=[]方式为模板绑定class
* 通过v-bind:class="{active:isActive,error:hasError}"绑定class，首先要在css中设置.active和.error,然后再Vue实例的data对象中设置isActive和hasError的布尔值;也可以直接传一个对象给class,即v-bind="classObj",再在data对象上直接赋值;
````
	data:{
    	classObj:{
        	static:true,
            active:true,
            error:true
        }
    }
````

* 你也可以通过传递数组的方式为class赋值v-bind:class="[staticClass,activeClass,errorClass]",此时你需要在data对象上为数组的元素的属性赋值:
````
	data:{
    	staticClass:'static',
        activeClass:'active',
        errorClass:'error'
    }
````

**无论是哪种方式，前提都是css中的class要先设定

>绑定style

html:
````
	<div id='app'>
    	<p v-bind:style="styleObj">Hello vue!</p>
    	<p v-bind:style="[styleObj,bgObj]">你好</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
            styleObj:{
                fontWeight:800,
                color:'red'
            },
            bgObj:{
                width:'100px',
                height:'80px',
                background:'#ccc'
            }
        }
    })
````

* 绑定style到模板的方法有两种, 一是v-bind:style="styleObj",然后在data对象上定义styleObj;而是可以通过数组方式为style传入多个样式对象

2.6

下面将详细介绍v-if、v-for和v-on指令
>条件渲染

html:
````
	<div id='app'>
    	<p v-if="ok">Hello vue!</p>
    	<p v-else>你好Vue!</p>
    	<template v-if="motto">
        	<h1>Steve Jobs</h1>
        	<p>motto:stay hungry ! stay foolish</p>
    	</template>
    	<p v-show="ok">Show Me</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
            ok:true,
            motto:true
        }
    })
````

* 通过v-if和v-else指令实现条件渲染，其中v-if="value"的value要在data对象中赋布尔值,v-if支持&lt; template &gt;语法
* v-show="value"是另一种条件渲染的方法;

**v-if和v-show的区别

* v-if是真实的条件渲染，当进行条件切换时,它会销毁和重建条件块的内容;
* v-show的田间切换是基于css的display属性,所以不会销毁和重建条件块的内容;
* 当你频繁需要切换条件时,推荐使用v-show;否则使用v-if;

>列表渲染

html:
````
	<div id='app'>
    	<ol>
        	<li v-for="book in books">
            	{{book.name}}
        	</li>
    	</ol>
    	<ul>
        	<li v-for="(food,index) in foods">
            	{{index}}---{{food}}---{{delicious}}
        	</li>
    	</ul>
    	<ul>
        	<li v-for="(value,key,index) in object">
            	{{index}}·{{key}}·{{value}}
        	</li>
    	</ul>
    	<div>
        	<span v-for="n in 10" style="margin-left: 5px;">{{n}}</span>
    	</div>
    	<span v-for="n in evenNumbers" style="margin-left: 5px;">{{n}}</span>
    	<span v-for="n in odd(counts)" style="margin-left: 5px;">{{n}}</span>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
            delicious:'delicious',
            books:[
                {name:'HongLouMeng'},
                {name:'SanGuo'}
            ],
            foods:[
                'tomato',
                'potato',
                'ice cream'
            ],
            object:{
                name:'lilei',
                age:'18'
            },
            numbers:[1,2,3,4,5,6,7,8,9,10],
            counts:[1,2,3,4,5]
        },
        computed:{
            evenNumbers:function () {
                return this.numbers.filter(function (number) {
                    return number%2 === 0;
                })
            }
        },
        methods:{
            odd:function (counts) {
                return counts.filter(function (count) {
                    return count%2 === 1;
                })
            }
        }
    })
````

* v-for指令能够让我们循环渲染列表型数据,数据放在data对象中,类型可以如下:

html:
````
data:{
	//数字数组
    numbers:[1,2,3,4,5,6,7,8,9,10],
    counts:[1,2,3,4,5],
    //字符串数组
    foods:[
        'tomato',
        'potato',
        'ice cream'
    ],
    //对象数组
    books:[
        {name:'HongLouMeng'},
        {name:'SanGuo'}
    ],
    //对象
    object:{
        name:'lilei',
        age:'18'
    }
}
````

* 根据不同类型的数据,v-for指令在模板中具体采用的语法如下:

````
//数据为数字数组
	<div>
		<span v-for="n in numbers">{{n}}</span>
	</div>
	//数据为字符数组
	<ul>
		<ol v-for="food in foods">{{food}}</ol>
	</ul>
	//数据为对象
	<ul>
  		<ol v-for="value in object">{{value}}</ol>
	</ul>
	//或者
	<ul>
  		<ol v-for="(value,key,index) in object">{{index}}.{{key}}.{{value}}</ol>
	</ul>
	//数据为对象数组
	<ul>
  		<ol v-for="car in cars">{{car.name}}</ol>
	</ul>
````

* 在v-for块中,我们拥有对父作用域属性的完全访问权限

2.7 事件监听
>简单的事件监听----直接在指令上处理数据

html:
````
	<div id="app">
    	<p v-on:click="counter+=1">{{counter}}</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
           counter:0
        }
    })
````

>复杂的事件监听----在methods对象定义回调函数

html:
````
	<div id="app">
    	<p v-on:click="greet">{{message}}</p>
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
           message:"Hello vue!"
        },
        methods:{
            greet:function (event) {
                alert(this.message)
            }
        }
    })
````
>事件修饰符----调用事件对象函数的快捷方式

````
  <div v-on:click.prevent="greet">1</div>//等价于event.preventDefault()
  <div v-on:click.stop="greet">2</div>//等价于event.stopPropagation()
  <div v-on:click.capture="greet">3</div>//等价于事件回调函数采用捕获阶段监听事件
  <div v-on:click.self="greet">4</div>//等价于event.target
````
>按键修饰符----按键事件的快捷方式

````
	常见按键别名包括：
	- enter
	- tab
	- delete
	- esc
	- space
	- up
	- down
	- left
	- right
````
2.8表单控件绑定

>文本控件

html:
````
	<div id="app">
    	<p>{{message}}</p>
    	<input type="text" v-model="message">
	</div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
           message:"Hello vue!"
        }
    })
````

* 通过v-model指令可以实现数据的双向绑定，即View层的数据变化可以直接改变Model层的数据,而Model层的数据改变也可以直接反映在View层;
* 上述代码v-model="message"使得input的value属性和message属性绑定,在输入框输入值,即改变value同时也改变message;

>单选控件

html:
````
    <div id="app">
        <input type="radio" id="man" value="man" v-model="picked">
        <label for="man">Man</label>
        <br>
        <input type="radio" id="woman" value="woman" v-model="picked">
        <label for="woman">Woman</label>
        <div style="margin-left: 10px;"></div>
    </div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
           message:"Hello vue!",
           picked:'man'
        }
    })
````

* v-model指令绑定data对象的picked属性,该属性默认指向type='radio'的input的value;

>复选框

html:
````
    <div id="app">
        <input type="checkbox" id="SGYY" value="SGYY" v-model="checked">
        <label for="SGYY">Man</label>
        <br>
        <input type="checkbox" id="HLM" value="HLM" v-model="checked">
        <label for="HLM">Woman</label>
        <div style="margin-left: 10px;">Checked Name:{{checked}}</div>
    </div>
````
Javascript:
````
    var app = new Vue({
        el:'#app',
        data:{
           checked:[]
        }
    })
````

2.9组件
>组件可以扩展HTML元素,封装可重用的代码。在较高层面上,组件是自定义元素,通过Vuefa.component()接口将大型应用拆分为各个组件,从而使代码更好具有维护性、复用性以及可读性
>
>
>
>注册组件

````
	<div id="app">
   		<my-component></my-component>
    </div>

	Vue.component('my-component',{
  		template:'<div>my-first-component</div>'
	})

	var app = new Vue({
  		el:'#app',
  		data:{
  		}
	})
````

* 注册行为必须在创建实例之前;
* component的template接口定义组件的html元素;

>局部注册组件

html:
````
    <div id="app">
        <my-component>
            <heading></heading>
        </my-component>
    </div>
````
Javascript:
````
    Vue.component('my-component',{
        template:'<div>my-first-component</div>'
    })
    var Child = {
        template:'<h3>Hello vue!</h3>'
    }
    var app = new Vue({
        el:'#app',
        components:{
            'my-component':Child
        }
    })
````

* 自定义模板将会销毁之前的DOM结构，重新生成模板结构，所以&lt;heading&gt;会消失不见
* 可以定义一个子组件，在实例的components接口中将子组件挂载到父组件上，子组件只在父组件的作用域下有效

>**特殊的DOM模板**将会限制组件的渲染

像这些包含固定样式的元素`<ul>,<ol>,<table>,<select>,`自定义组件中使用这些受限制的元素时会导致渲染失败;
通常解决方案是使用特殊的is属性：
````
	<table>
		<tr is="my-component">
	</table>
````
>**创建组件的data对象必须是函数**

html:
````
    <div id="app">
        <counter></counter>
        <counter></counter>
        <counter></counter>
    </div>
````
Javascript:
````
    Vue.component('counter',{
        template:'<button @click="count+=1">{{count}}</button>',
        data:function () {
            return {
                count:0
            }
        }
    })
    var app = new Vue({
        el:'#app'
    })
````

* 在组件当中定义的数据count必须以函数的形式返回;

>**使用Props实现父组件传递数据**

html:
````
    <div id="app">
        <child some-text="hello"></child>
        <br>
        <child v-bind:some-text="message"></child>
    </div>
````
Javascript:
````
    Vue.component('child',{
        template:'<div>{{someText}}</div>',
        props:['someText']
    })
    var Child = {
        template:'<h3>Hello vue!</h3>'
    }
    var app = new Vue({
        el:'#app',
        components:{
            'my-component':Child
        },
        data:{
            message:'你好'
        }
    })
````

* 组件实例的作用于是孤立的，这意味着不能并且不应该在子组件的模板内直接引用父组件的数据.可以使用peops把数据传给子组件;
* 可以用v-bind动态绑定peops的值到父组件的数据中.每当父组件的数据变化时,该变化也会传导给子组件,注意这种绑定的方式是单向绑定;

>**父组件是使用props传递数据给子组件，但如果子组件要把数据传递回去则使用自定义事件**

html:
````
    <div id="app">
        <p>{{total}}</p>
        <button-counter v-on:incrementTotal></button-counter>
        <button-counter @increment="incrementTotal"></button-counter>
    </div>
````
Javascript:
````
    Vue.component('button-counter',{
        template:'<button v-on:click="increment">{{counter}}</button>',
        data:function () {
            return{
                counter:0
            }
        },
        methods:{
            increment:function () {
                this.counter+=1;
                this.$emit('increment')
            }
        }
    })
    var app = new Vue({
        el:'#app',
        data:{
            total:0
        },
        methods:{
            incrementTotal:function () {
                this.total += 1;
            }
        }
    })
````

* 父组件可以通过监听子组件的自定义事件,从而改变父组件的数据;
* 子组件每点击一次,触发increment函数,该函数在执行过程中通过$emit('increment')发出increment事件;
* button控件同时监听increment事件,每次发出该事件就改变父组件的total值;





















































































































