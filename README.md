---
	
---

# es5和es6语法

## let和var

![	](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305210731114.png)

### 变量作用域

变量在什么范围内有效/是可用的。主要是理解事件的处理流程，第一个是同步任务，第二个是异步任务，异步任务永远是晚于同一时间段的同步任务执行的。

var的作用范围是全局，这引起一些其他的问题，全局变量污染

```javascript
if(true) {
  var name = 'a';
}
console.log(name);
```

没有块级作用域，以前的写法是通过闭包的形式防止其他的函数随意篡改数据。闭包的形式就是利用了函数是一个作用域。

![image-20220305211806770](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305211806770.png)

说明上面的例子：对于有块级作用域的语言来说，每个console打印出来的i肯定是遍历的顺序的i，但是对于var定义的i来说，因为是没有块级作用域的，也就是所有的i都是用的同一个，for循环是执行在主线程上的同步任务，而回调函数是交给异步进程处理的被放入了异步任务队列的任务，只有同步任务完成后再去异步队列检查，又因为使用的是同一个i，所以当所有的都添加了监听事件后，此时的i已经变为了5，而不是每个console打印的是不同的顺序的变好了。可以寻找js的执行顺序等。

js将任务分为了同步和异步两种任务，对于常见的函数，监听事件，定时器等都是归类为异步人物的，也就是当js的执行器遇到了异步任务，那么就交给一一个名为异步进程的程序处理，然后据徐执行主线程中的同步任务，直到执行完成了主线程中的同步任务了，才会去异步线程分好的异步任务队列中去执行异步任务队列。

对于这个例子来说，先执行好所有的分配任务，也就是主线程执行了for循环，顺序安排好了之后，此时的i变为了4，接着再去生成所有的监听器，当所有的监听器（异步任务）都完成了以后，因为var定义的变量没有块级的作用域，所以所有的监听器都是使用的同一个i（先执行for同步任务，然后再分配好监听器，此时的监听器分配到的i就是4了）这就是没有块级作用域的坏处。

简单来说就是两个对象 father和son

生成father时，如果刚开始钱是500，son是还没有生成，此时的son被分配到了异步任务队列，当father的所有属性都完成了以后，如果此时的钱变为了1000，son再开始生成，此时son得到的是1000，而不是500了，相当于原有的for就是father，最后的i变为4，但是son此时才开始生成，所以每个son得到的i都是最后一个4，而不是遍历的序号。

# 高阶函数

filter 过滤/map 映射/reduce 汇总

- filter每遍历一个元素回调一次函数
- map每遍历一个元素回调一次函数
- reduce每遍历一个元素回调一次函数

高阶函数和链式调用，一定要掌握这三个高阶函数，第一个是过滤，第二个是映射，第三个是汇总。



# input的常用修饰符

懒加载，数字型，去除空格

- lazy修饰符，懒加载，只有当数据在失去焦点或者是回车时才会更新。
- number修饰符，将input输入框的数据默认当作数字类型来处理
- trim修饰符，即去掉input输入框数据两边的空格

![image-20220305231902843](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305231902843.png)

# 组件化思想

组件是一个一个模块，相当于面向对象过程中的对象，每一个对象完成一部分功能，所有的对象的组合完成了整体的功能，这就是组件化开发，而对于前端的过程，因为三种技术的复杂，所以将三种技术融合在一块，降低了复杂度，便于管理和维护。

![image-20220305232912441](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305232912441.png)

## Vue组件化
组件必须要有一个根标签，也就是vue组件中的所有内容都必须用一个根标签包裹

![image-20220305233355386](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305233355386.png)

## 组件开发

![image-20220305233543481](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305233543481.png)

- 创建组件

创建组件就是为了可复用，使用Vue的extend方法，传入一个对象，这个对象必须腰包含一个模板属性，模板属性其实也就是 这个组件的 html css js的写法。

```javascript
const component = Vue.extend({
  template: `
		<div></div>
	`
});
```

- 注册组件，使用vue的component方法

```javascript
Vue.component('组件名称', 组件对象);
Vue.component('hello', hello);
```

- 使用组件，其实也就是在需要的区域直接标记组件组件名称即可。

```javascript
<hello></hello>
```

这样整个组件的开发就完成了，接下来就是引用组件的过程。

![image-20220305235751360](Vue%E7%AC%94%E8%AE%B0.assets/image-20220305235751360.png)

### 全局组件和局部组件

![image-20220306000300755](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306000300755.png)

全局组件的注册方法就是使用Vue.component方法进行注册，局部组件的注册方式就是在实例或者时父级组件中的comjponents中进行注册即可。

全局组件的简写方法

直接在vue.component中第二个参数对象参数直接定义参数即可。

```javascript
Vue.component('cpn', {
  templaate: `
		<div>
			<h2>我是标题</h2>
		</div>
	`
});
```

局部组件的注册简写方法

````javascript
components: [
  'cpn2': {
  	template: ·
  		<div>
  			<h2>我是标题<h2>
  			<p>我是字段，哈哈哈</p>
  		</div>
  	·
  }
]
````

这种简写方式实际上还是调用了extend进行组件的创建，只是将组件的创建和组件的使用两个步骤结合在一块。

### 模板的分离化写法

![image-20220306002627215](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306002627215.png)

使用template标签，然后给定id，直接在创建组件中的template进行id挂载即可。


### 父子组件

![image-20220306000935486](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306000935486.png)

见到那来说就是自定义的组件，一个自定义的组件可以作为另外一个自定义组件的components中的一员，即一个组件可以当跟组件的子组件，同时又是另外一个自定义组件的父组件，对于自定义组件来说，有一个就是父组件有一个就是子组件。

## 组件的作用范围

组件只有在已经注册了的范围内或者时全局组件才可以使用，否则vue无法解析组件标签，如一个自定义组件a在另外一个自定义组件b中注册了，而另外一个组件b在实例中注册了，那么组件b可以在实例中使用，但是即使组件a在组件b中注册了，无法直接在实例中使用组件a的标签，组件a的标签必须包含在组件b中，因为组件都是有作用范围的。但是组件a即可以在组件b中注册，同时也可以在实例中注册，不影响组件的使用。

## 组件的访问范围

组件不能直接访问实例的数据。

![image-20220306002809925](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306002809925.png)

创建组件时，组件可以带有自己的数据方位，也就是可以带着data数据，这些数据的存储范围也是data，但是此时的data是一个函数，而不是一个属性。

在**实例**中，data是作为一个属性的，也就是是一个对象，但是对于组件中的data，只能是一个**函数**，返回的是一个对象，这样才可以带有数据的返回。

```javascript
实例
data: {
  ...
}

组件
data() {
  return {
    title: '世界你好',
  }
}
```

组件中的data之所以是函数，是为了组件重复使用而引起的问题，如果组件的data是一个对象，那么所有的组件实例中都是共用的同一个数据，而函数具有块级作用域，所以组件的可复用性就不会出现问题，这就是组件中的data为什么腰是函数。

# 组件通信

## 解释

通常来说，页面展示的数据由根实例或者是父组件进行请求数据，然后将数据交给子组件进行数据展示。为什么不是子组件进行请求呢？

- 首先，组件是一个可复用的模块，如果在组件中固定了请求的路径，那么整个模块只能复制粘贴创建一个新的模块来替代。
- 其次，不方便管理，数据从父级流向子级，能够控制数据的显示，哪一些需要显示，哪一些不需要显示，都能够在父级组件或者实例中进行控制。
- 按照前后端分离的趋势或者是微服务的架构模型，如果使用子组件进行通信，那么每次子组件请求数据都需要辨认身份，如果子组件越来越多，这样造成的身份辨的消耗就大，而如果使用的是根实例或者是父组件依次通信得到所有子组件的数据信息，只需要辨认一次身份即可。 

## 组件通信

![image-20220306074331500](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306074331500.png)

通常将根实例作为一个父组件进行数据的请求。

### 父级向子级传送数据 props
约定：父组件是根实例，子组件是cpn

- 父级向子级传送数据使用的是 `props`（properties）属性，整个属性写在子组件中，和template同级。
- 在子组件的props中，定义变量，`['cmessage', 'cmovies']`
- 然后在子组件中的需要绑定数据的地方使用v-bind直接绑定父组件响应的数据即可。`<cpn :cmessage="message"></cpn>` 整个语法就是将子组件的 cmessage绑定到父组件的message，使用父组件中的message数据。注意这里一定是在父组件中的子组件标签中进行绑定，否则数据无法传输。但是在子组件使用数据时，就不需要进行绑定，比如

```html
子组件中直接使用即可
<div>{{ cmessage }}</div>


父组件中，子组件标签名中应该要绑定父组件的数据
<cpn :cmessage="message"></cpn>
```

简单来说父组件向子组件传送数据使用的是子组件的props属性，props相当于自定义的属性，然后子组件使用v-bind给自定义的属性绑定上父组件中的相应的数据，子组件即可使用相应的数据了。相当于props是一个子组件的数据仓库，子组件若果想要使用数据，那么子组件使用使用v-bind去父组件中取出来放在自己的props仓库中，这样才可以使用。

总体流程：子组件建好props（仓库），仓库中会留有存放各种数据的地方，然后按照v-bind（单号）从父组件中取出对应的数据放在自己的仓库中，绑定什么就取出什么数据。绑定了（取出了数据以后）就可以在子组件的结构中使用相应的数据了。

#### props写法

![image-20220306080227891](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306080227891.png)

prps除了可以使用对象形式

```javascript
props: ['cmessage','cmovies']
props: {
  // 可以限制数据类型
  // 可以提供一些默认值
  cmessage: {
  	type: String,
  	default: 'message',
  	required: true
  }
}

props: {
	//数据类型可以是一个多选择的数据
	cnum: {
		type: [Number, String],
		default: '0',
		required: false;
	}
}

props: {
	//如果数据类型是对象或者数据，那么默认值应该使用default函数进行隔离，而不是使用default属性
	cnums: {
		type: Array,
		default() {
			return ['Hello'，‘World’]
		},
		required: false;
	}
}

props: {
	cobj: {
		type: Object,
		default() {
			return {'name','Hello'}
		},
	}
}

props: {
	// 可以使用自定义类型
	cobj: {
		author: Person,
		default() {
			return {
				name: ''
			}
		},
		required: false;
	}
}
```
通常在开发中，props就是使用对象类型，因为对象类型提供的功能更加的更加的丰富，通常使用的是type，default，required。

注意：如果数据类型是对象或者数组，为了块级区分不同的作用域，这种堆类型的数据都是需要使用函数来进行区分的，因为函数由作用域，这样就可以隔离开不同的组件实例，使用的数据就不会是同一个。

#### 注意驼峰命名法
注意html标签中不识别大小写，所以Vue再识别中，如果是绑定数据位置， cMessage应该写成 `c-message`，这样才不会错。除了绑定数据的位置，其他的位置不需要改变，应该不识别大小写是标签引起的，只需要在绑定数据的位置进行转换即可。尽量避免使用驼峰命名法，因为html不识别大小写。

### 子组件发送信息到父组件 $emit("事件名称")

父组件传递方法引用给子组件，子组件调用并传入参数，所以相当于父组件接受了子组件的数据。
简单来说就是：子组件使用$emit("事件名称")发送了一个信息，父组件然后在自身中的子组件标签中，进行监听，最后对事件进行处理。

```html
子组件中
methods: {
	btnClick(item) {
		this.$emit("itemClick",item) // 子组件监听到自身被点击了，那么发送信息出去,同时可以将参数发出去。
	}
}


父组件中
<cpn @item-click="cpnClick"></cpn>



以前监听click事件
<div @click="divClick"></div>

methods: {
	divClick(){
		console.log("click");
	}
}


现在变为了监听自定义的事件 item-click，然后进行处理即可
```
> 以前总是通过 v-on @来监听事件，事实上，@不仅可以间厅柜默认的click事件等，还可以监听自定义的事件，此时在父组件中，监听子组件发送信息这个事件，这个事件就是 @itemClick，只是驼峰转换以下变为了 @item-click。而在以前是用@click时，都是@click=“自身组件事件处理函数”类似流程，这也就是父组件中的cpn标签监听了事件item-click，然后在父组件中进行处理
> 
总的流程： 子组件监听自身被点击-调用方法发送信息给父组件(`$emit('事件名称a')`)-父组件监听子组件的自定义事件（事件名称a），然后父组件根据不同的事件做出相应的处理，处理流程就是在父组件的methods中。

#### 案例
实现父子通信 
子组件中有两个input输入框，子组件中能够修改父元素的data的树，第一个输入框的数字时第二个输入框的百分之一，也就是第二个输入框是第一个输入框的一百倍。并且两个输入框都能够修改父元素的数值。

子组件一般不能直接修改父组件中传过来的数据，就是不能直接修改props的数据，而是通过发送信息给父组件，然后父组件修改父组件中的数据，因为props是绑定了父组件的数据，所以props也会跟着变化，即先发送信息给父组件，让父组件自己去修改，子组件中的数据跟着变化就是了。

![msedge-20220306110526](vue%E7%AC%94%E8%AE%B0.assets/msedge-20220306110526.gif)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="app">
    num1: {{num1}} || num2: {{num2}}
    <capp :number1="num1" :number2="num2"
          @num1change="num1change"
          @num2change="num2change">
    </capp>
  </div>

  <template id="capp">
    <div>
      <h2>props-number1/num1: {{number1}}</h2>
      <h2>v-model: {{dnum1}}</h2>
      <input type="text" v-model="dnum1" @input="dnum1input">
  
      <h2>props-number2/num2: {{number2}}</h2>
      <h2>v-model: {{dnum2}}</h2>
      <input type="text" v-model="dnum2" @input="dnum2input">
    </div>
  </template>

  <script src="vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        num1: 1,
        num2: 0
      },
      methods: {
        num1change(num) {
          this.num1 = parseFloat(num);
        },
        num2change(num) {
          this.num2 = parseFloat(num);
        }
      },
      components: {
        capp: {
          template: '#capp',
          // 父传子通信
          props: {
            number1: {type: Number,default: 0},
            number2: {type: Number,default: 0,}
          },
          data() {
            return {
              dnum1: this.number1,
              dnum2: this.number2
            }
          },
          methods: {
            dnum1input(event) {
              this.dnum2 = event.target.value * 100;
              this.$emit('num1change',event.target.value);
              this.$emit('num2change',this.dnum2);
            },
            dnum2input(event) {
              this.dnum1 = event.target.value / 100;
              this.$emit('num1change',this.dnum1);
              this.$emit('num2change',event.target.value);
            }
          }
        }
      }
    });
  </script>
</body>
</html>
```
## 父子组件直接访问

![image-20220306111819670](vue%E7%AC%94%E8%AE%B0.assets/image-20220306111819670.png)

$refs最常使用，直接可以拿到对象和对象的属性，必须在组件上面加上 ref="唯一名"，相等于是一个css类名一样。

# 插槽slot

插槽说白了就是组件的组件，是组成组件的组件，可以选可以不选，可以根据页面的选择来使用一部分插槽。

![image-20220306114018598](vue%E7%AC%94%E8%AE%B0.assets/image-20220306114018598.png)

![image-20220306114444701](vue%E7%AC%94%E8%AE%B0.assets/image-20220306114444701.png)

![image-20220306114457142](vue%E7%AC%94%E8%AE%B0.assets/image-20220306114457142.png)

## 具名插槽

如果直接插入内容，只会将无名的插槽进行替换，具名的插槽不会进行替换。这位组件化开发提供了很好的索引，如果没有指定插入哪个槽，那么会取代无名的插槽，如果制定了具名的插槽，那么只会插入具名的插槽。

![image-20220306115116177](vue%E7%AC%94%E8%AE%B0.assets/image-20220306115116177.png)

插槽不仅仅可以替换相同的元素，不同标签的元素也是可以替换的。

## 作用域
template的相关变量是在自定义的组件中寻找的，但是只要是在根实例中的各种标签各种变量，都是根实例的变量标签而不是组件的变量了，这就是作用域i的概念，哪一个组件的变量寻找都是在自身的数据仓库中寻找，而不是在其他的地方寻找，即使可以通过父子通信或者是兄弟元素的通信，那也是先转换为自己的变量空间的内容了，然后再去使用，这是作用域的限制。

![image-20220306120102877](vue%E7%AC%94%E8%AE%B0.assets/image-20220306120102877.png)

## 作用域插槽
如何从父组件中拿到子组件的变量？

![image-20220306121422689](vue%E7%AC%94%E8%AE%B0.assets/image-20220306121422689.png)

### 使用方法

![image-20220306121138505](vue%E7%AC%94%E8%AE%B0.assets/image-20220306121138505.png)

父组件从子组件中拿到内容，但是内容的显示是由父组件来绝对能够的。简单来说就是：父亲想要拿到儿子的红包，并且由父亲决定这些红包放在哪里并且怎么放。

# 模块化

## 模块化简介

![image-20220306122510854](vue%E7%AC%94%E8%AE%B0.assets/image-20220306122510854.png)


## 早期实现模块化

![image-20220306123119658](vue%E7%AC%94%E8%AE%B0.assets/image-20220306123119658.png)



## ES6模块化
### 导入导出

![image-20220306133435654](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306133435654.png)

- 可以导出对象，对象中可以包含函数对象和变量
- 直接导出变量
- 直接导出函数

关键是使用export关键字

### 各种导出方式

![image-20220306133728297](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306133728297.png)

### export default

![image-20220306134022310](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306134022310.png)

### import导入

![image-20220306134206979](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306134206979.png)

### 常用的commonjs模块化形式

导出

```javascript
module.exports = {	
	导出的对象，属性，函数
}

导入，最好后缀都加上.js，防止一些奇怪的问题。
const {导入的对象名称} = require('js的相对路径'); 
```



# webpack

如果需要配置文件 [webpack](webpackjs.com)

## webpack简介

![image-20220306134354755](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306134354755.png)

### 前端打包工具

![image-20220306134557492](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306134557492.png)

### 对比

![image-20220306134646248](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306134646248.png)

### webpack打包工具使用5.0

## JS打包

`5.0版本webpack --entry ./src/main.js -o ./dist --mode=development`

![image-20220306145614243](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306145614243.png)

在模块化开发前端的业务时，最重要的是首先先将webpack.config.js搭建好，直接使用npm init就可以搭建好webpack.config.js了

定义脚本，优先使用本地的webpack版本，如果直接写webpack，一般都是全局的webpack，一般都是使用项目的webpack。增加了这个脚本，优先是使用项目的版本进行构建，否则使用全局的版本进行构建。

```js
{
  "name": "first",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode=development"
  },
  "author": "luckydog",
  "license": "ISC"
}
```

## CSS、less打包

webpack能够满足js的打包需求，但是对于其他的css文件，图片等没有这个能力，所以需要加载其他的配置，其他的配置就称为loader。

css只负责将css文件加载，不负责显示界面等渲染。

![image-20220306152450761](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306152450761.png)

- css-loader
- style-loader
- less-loader,，less

```shell
css-loader
npm install --save-dev css-loader --legacy-peer-deps
style-loader
npm install --save-dev style-loader --legacy-peer-deps
less-loader
npm install --save-dev less-loader less --legacy-peer-deps
```

webpack.config.js

```javascript
const path = require('path')

module.exports = {
  entry: './src/main.js',
  output: {
    // path: '',动态的获取当前的路径
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
    publishPath: 'dist/' // url的配置
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },
      {
        test: /\.less$/,
        use: [{
          loader: 'style-loader' // 将样式文件加载到js
        },{
          loader: 'css-loader' // 将css转换为commonjs
        },{
          loader: 'less.loader' // 将less转换为css
        }]
      }
    ]
  }
}
```

## 资源打包

- url-loader
- file-loader

```shell
npm install --save-dev url-loader --legacy-peer-deps
```

Es6语法处理

![image-20220306163348372](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306163348372.png)

## 引入Vue

![image-20220306163633143](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306163633143.png)

```javascript
输入 依赖即可
import Vue from 'vue'
```

报错原因

![image-20220306164425374](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306164425374.png)

解决方法

```javascript
import Vue from 'vue/dist/vue.js' // 导入时选择这个导入即可
```

## el和template

![image-20220306165251643](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306165251643.png)

使用组件替代原有的结构。

目的是为了简化基本的架构。最后简化成template和script和style三个架构。

![image-20220306170142130](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306170142130.png)

- 第一步：如果根实例中同时出现了 el 和 template两个属性，那么el挂载的标签会被后来的template覆盖掉，这是抽出template标签的第一步。



1. 普通的vue程序

```html
<div id="app">
</div>

<script>
  new Vue({
    el: '#app',
    data: {},
    methods: {}
  });
</script>
```

2. 使用组件覆盖原标签

当el和template同时出现时，template会将el挂载的标签覆盖，也就是此时剩下的只是template中的标签。根实例同样的也可以看成一个组件。

```html
<div id="app">
</div>

<script>
  new Vue({
    el: '#app',
    template: `
    <div>
    <h2></h2>
      </div>
    `,
    data: {},
    methods: {}
  });
</script>

变化后，此时的el没有挂载
<div>
  <h2></h2>
</div>

<script>
  new Vue({
    el: '#app',
    data: {},
    methods: {}
  });
</script>
```

3. 既然可以覆盖掉原有的标签，那么抽出template标签作为组件，然后使用template属性覆盖。

```html
  <script>
//   	创建对象，定义组件，将原有的数据都抽成组件，最后实例中的template就是组件的标签名
    const App = {
      template: `
        <div>
          <h2></h2>
        </div>
      `,
      data() {},
      methods: {}
    };
    new Vue({
      el: '#app',
      template: '<App />',
      components: { // 抽出组件的同时，在根实例中注册组件
        App
      }
    });
  </script>
```

因为同时出现template和el时，会覆盖掉原有的挂载对象。所以最后页面就生下了 <App /> 同时因为实在template里面，所以组件会生效。

4. 因为定义的const App过程都是js过程，所以单独抽出来，同时声明导出。

```javascript
在app.js中
export default {
  template: `
  <div>
    <h2></h2>
  </div>
  `,
  data() {},
  methods: {}
}

剩下来的原有的js中
new Vue({
  el: '#app',
  template: '<App />',
  components: {
    App
  }
});
```

5. 导入app.js中的对象即可

```javascript
import App from './app.js'
new Vue({
  template: '<App />',
  components: {
    App
  }
});

原有的app.js
export default {
  template: `
  <div>
    <h2></h2>
  </div>
  `,
  data() {},
  methods: {}
}
```

这就是组后脚手架的模型，直接导入 `import App from 'vue'`这个App就是根实例，也是根级别的组件。

6. 最后组件的各种封装。

使用export default 即可每一个单独的组件都可以导出，这就是优势。之前明确过，export default 只能有一个，并且export default 是可以起别名的组件。这就为使用者提供了极大的遍历。

```vue
<template>
  <div>
    <h2></h2>
  </div>
</template>

<script>
  export default {
    name: 'App'
  }
</script>

<style scoped>

</style>
```

各种脚手架引用的时候使用的方法 `import` 

```javascript
import App from './app.js'
const app = new Vue({
  template: '<App />',
  components: {
    App
  }
});
```

最终抽出来的就是一个一个的组件，也就是一个一个的模块

## 总结

以上即使脚手架的演进，也是为什么模块化越来越受欢迎的由来，让每个开发者自由的编写自己的代码，并且结构非常清晰，除了单独的组件外，单个组件的模板分为模板，脚本的跟为脚本，样式的分为样式。

# 插件

![image-20220306183202137](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306183202137.png)

## 打包HTML文件，生成默认的index文件。

![image-20220306183941714](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306183941714.png)

在webpack.config.js中进行插件的配置。一些插件能够很好的帮助我们进行代码的编写，例如html的生成器，所以运用好插件是对我们效率的提高。

webpack.config.js是配置各种webpack工具的地方，而package.json就是配置各种依赖的版本或者是脚本命令的地方。

- packagejson创建项目时就应该创建
- webpackconfig在创建项目时使用 npm init就可以创建使用。

## js压缩插件

![image-20220306184636794](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306184636794.png)

对于js来说，进行压缩后会明显的减少体积，这样在加载的时候会更加的快速。开发阶段一般不进行js的压缩，为了方便做好检查代码。

## 搭建本地服务器

![image-20220306185456215](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306185456215.png)

详细的可以参考webpack官网，这种配置只是一次性的，不需要经常配置，所以不需要记住，也记不住。

![image-20220306185850912](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306185850912.png)

# 脚手架

![image-20220306191441921](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306191441921.png)

## 安装使用

`npm install -g @vue/cli`

![image-20220306191716693](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306191716693.png)

## 详细使用

### complier和runtimeonly区别
对比图

![image-20220306201138297](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306201138297.png)

complier是注册components，但是runtimeonly是直接用render函数进行渲染。

### 渲染过程的选择

![image-20220306202300257](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306202300257.png)

1. 第一种渲染是从头到尾进行编译渲染的，每一个过程都经历
2. 第二种是直接从render函数渲染成vdom，然后展现成真实的dom也就是ui。

在第一种渲染方式中，所有的template标签是等到了运行时才是开始编译成抽象语法树，然后再进行渲染的，抽象语法树其实就是一个一个的对象组成的交叉连接的网，然后使用render函数将抽象语法树渲染成vdom，最后才渲染成真实的ui。而第二种渲染方式是 ： 在编写代码时，制定了一开始就需要编译将书写的代码编译成对象，这就是区别，但是对于第一种来说，比较好理解，比较容易动整体的流程。

## vue CLI3

![image-20220306203939254](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306203939254.png)

目录相关

![image-20220306205902024](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306205902024.png)

配置文件

![image-20220306211158461](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306211158461.png)

## 箭头函数 重点

箭头函数是一个简写的函数方式。定义函数的方式有很多种，只需要记住，在JS种往往函数就是对象，每一个东西都可以看成是对象，一定要注意，如果需要调用函数，只是调用函数调用对象的话不需要括号，如果加上了括号，就变为了是执行给定的函数，并且将返回值返回给调用者。这两个意思是不同的，第一个是执行调用函数即可，第二个是执行了函数后将函数的返回结果给调用者，如果没有返回值，那么就是undefined。

```javascript
a: function(){
  
}
function a(){
  
}
const a = function() {
  
}
es6种箭头函数的写法其实和第三种差不多。之不多将function关键字用将头代替了

函数的参数用括号显示，function省略，参数后哦用箭头代替，如果是一个参数，那么可以省略括号
const a = (参数猎豹) => {
  函数体
}
const a = num => {
  函数体
}
const a = () => {
  函数体
}

函数代码块中只有一行代码
const a = (num1, num2) => {
  return num1 + num2
}
const a = (num1, num2) => num1+num2 自动返回

注意：大括号和return要么都写要么都不写，只写一个出问题


setTimeout(function() {
  console.log(this);
},1000);
setTimeout(()=>{
  console.log(this);
},1000);

const obj = {
  setTimeout(function(){
    console.log(this);
  },1000);
  
  setTimeout(()=>{
    colsole.log(this); 除了这个打印的obj外，其他的都打印的是window 
  },1000);
}


箭头函数䣌this指向问题
箭头函数体内的this对象，就是定义该函数时所在的作用域指向的对象，而不是使用时所在的作用域指向的对象。

摘自 阮一峰 es6入门：对于普通函数来说，内部的this指向函数运行时所在的对象，但是这一点对箭头函数不成立。它没有自己的this对象，内部的this就是定义时上层作用域中的this。
```

# vue-router

路由作为动词时，是一个转发和发送的过程。在vue中，vue-router是一个插件，vue中的插件都需要使用Vue.use(插件)来进行安装。然后在生成对象。vue-router就是将各种path和组件一一对应起来，将映射的path和组件使用对象包裹起来，这样就形成了映射。直接查询对象的path，就能够得到对象的组件是什么。

![image-20220306213824537](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306213824537.png)

路由作为名词的时候是一个到达的路径

![image-20220306213812596](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306213812596.png)

## 后端路由阶段

![image-20220306214606609](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306214606609.png)

## 前端路由

![image-20220306220345624](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306220345624.png)

前端路由不是说每次生成一个路由就向服务器请求一次，而是在刚开始就将所有的静态资源请求下来，然后生成现实的页面，在生成路由的时候就对已经下载的静态资源进行渲染，此时向后端发送请求数据，这个才是前端路由阶段，也就是一开始所有的静态资源都已经下载好了，当用户点击了某个界面，那么前端路由将该页面的资源进行渲染，然后再去向后端服务器发送请求请求数据。一旦url发生改变，不是说向服务器再次请求，而是直接在本地渲染资源，然后向后端服务器请求数据，不在有关于请求静态资源的流程了，除了第一次请求静态资源服务器外，其他时间只要资源不缺失，那么就不会再发生请求静态资源的问题，这样网页的加载速度也会变快，因为后端只是数据的发送，而不是静态资源等大文件的传送。

## 修改网页的url不发生刷新

- 修改网页的hash，通过location对象的hash进行修改

![image-20220306221458821](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306221458821.png)

- 利用H5提供的history对象进行修改 history.pushState(三个参数)

用这个可以去#，面试会用到的哦：history.go(-1)

在最新系统的开发中，ios不再支持以上的几个函数，所以app的开发者应该注意的就是这几个函数的使用。否则函数不在生效，俺么页面的监听无法跳转，并且页面无法渲染。

![image-20220306222305965](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306222305965.png)

## 安装使用

各种路由简介

![image-20220306222403656](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306222403656.png)

路由使用：使用脚手架已经自动创建了路由。

![image-20220306222429201](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306222429201.png)

## 使用

在cli2中，需要自己手动创建路由对象，步骤

- 在router中导入Vue和VueRouter `import VueRouter from 'vue-router' `
- Vue安装使用VueRouter `Vue.use(VueRotter)`
- 最后创建VueRouter对象

VueRouter中，包含了重要的属性，第一个是 routes 即路由映射关系，简单来说是路由表

```javascript
const routes = [] 定义一个路由表，即路由映射关系
const router = new VueRouter({
  routes：routes  将创建的路由表传入router生成对象中，便于路由对象管理，这里的路由映射对象和路由对象是不一样的对象功能。
})

export default { // 使用默认的 export default是的其他的开发者能够自定义的设定别名
	name: 'Router'
}

导出路由后，需要在main.js 中使用路由，这样才能将路由对象挂载到实例上。
```

![image-20220306223719391](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306223719391.png)

## 总体流程

- 如果router文件夹中没有index.js文件，那么创建index.js文件。
- 在index.js文件导入 vue 和 vue-router两个组件
- 使用Vue.use(VueRoouter); 堆VueRouter进行安装注册
- 创建VueRouter对象，并且将VueRouter进行导出
- 挂载到根实例上，也就是将VueRouter注册到根实例的Components中，这样路由的映射管理才能生效

- 组件的显示需要用到的是rooter-link和router-view两个标签页，这两个标签可以让组件渲染出来，第一个标签是a标签的变形体，有属性to，标记去往那个路由，路由和组件进行绑定。
- router-view相当于一个占位符，能够在路由跳转后显示当前的组件元素，相当于是一个空位置，决定组件的内容显示的位置，和插槽类似。

![image-20220306233522619](Vue%E7%AC%94%E8%AE%B0.assets/image-20220306233522619.png)

最简单的方式来理解就是： router-link标签是决定显示什么内容的，而router-view是决定实现在页面的位置的，两个都是和其他标签统计的存在，不会被覆盖。

## 今天总结
1. vue应用理解
每个vue实例都是一个被打包好的页面，只是多了js后可以进行数据请求的通信，每一个vue应用都是一个单页面的，但是即使是单页面，因为插槽作用的存在随时可以替换其中的元素，所以表现出来的结果就是显示出多个页面的存在。
2. 父子组件之间的通信
父组件向子组件中传递信息使用的是 ： 子组件的props属性。简单来说，就是父组件的数据需要给子组件，那么子组件中应该有一个地方存放父组的数据，而因为数据不仅仅只是一个，所以需要在父组件的标签中的子组件的标签名中，绑定props的变量和父组件中的哪个变量。这样才可以绑定相应的数据。
3. 插槽
4. webpack和脚手架CLI

webpack主要的内容就是两个，第一个就是package.json第二个就是webpack.config.js两个，在packagejson中可以定义依赖的版本信息，或者是脚本的执行，第二个config.js中可以配置插件，等等作用。

5. 路由

路由这一部分和后端的路由不一样，因为前端的路由仅仅是因为路由的变化但是不产生新的静态资源请求，这是因为vue是一个单页面的富应用的存在。安装路由，定义路由规则，导出路由，挂载路由到根实例上。最后使用touter-link和router-view两个标签进行路由的显示和跳转。

## 使用history模式

```javascript
export default new Router({
  routes: routes,
  mode: 'history'
})
```



![image-20220307071615190](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307071615190.png)

router-link默认点击时会修改class属性，其中router-link-acitve是可以修改默认活跃状态的。如果不想使用这个类名，可以在路由对象生成中，添加一个属性 linkAcitveCalss： ‘活跃的类名’

![image-20220307072021075](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307072021075.png)

![image-20220307072328589](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307072328589.png)

## 具体的每一个触发跳转

除了可以使用vue给定特定标签router-link标签的to属性外，还可以使用自定义的js代码跳转。router-link有一个特殊的标签，因为刚开始router-link默认的会被渲染成a标签，所以如果想要渲染成其他的标签，直接在router-link上加上tah属性即可，属性值就是被渲染的标签名

`<router-link to="" tag="button"></router-link>`

这样router-link会被渲染成 button标签。

![image-20220307073056796](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307073056796.png)

`this.$router.push('/home').catch(err=>{})`

但是这个不能重复跳转，也就是当前的页面path不能跳转到当前的path，否则会出现出错。如果连续跳转报错，但是没有影响业务的情况下可以使用上面的代码，将错误捕捉下来然后给定一个空函数对象，就是不做任何处理，但是能够将错误捕捉下来。这也是一个箭头函数的使用。

![image-20220307073734559](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307073734559.png)

不要直接使用history对象进行pushState的转换，vue应用不要绕过了bue-router进行直接跳转，尽量将所有的路由都交给vue-router管理跳转。

![image-20220307073822297](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307073822297.png)

```vue
<template>
  <div id="app">
    <!-- <router-link to="/home">首页</router-link>
    <router-link to="/about">关于</router-link> -->
    <button @click="homeClick">首页</button>
    <button @click="aboutClick">关于</button>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    homeClick() {
      this.$router.push('/home').catch(err => {})
    },
    aboutClick() {
      this.$router.push('/about').catch(err => {})
    }
  }
}
</script>

<style>

</style>
```



router中的内容

```js
import Vue from 'vue'
import Router from 'vue-router'
import Home from '../components/Home.vue'
import About from '../components/About.vue'
import User from '../components/User.vue'

Vue.use(Router)

const routes = [
  { path: '/', redirect: '/home' },
  { path: '/home', component: Home },
  { path: '/about', component: About },
  { path: '/user', component: User }
]

export default new Router({
  routes: routes,
  mode: 'history'
})
```

各种导入可以使用箭头函数的简写形式，因为箭头函数就是默认会带有返回的，即使是undefined。

`{path: '/user', component: ()=>import("../components/User.vue")`

## 动态路由

![image-20220307082426469](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307082426469.png)

使用 `:params` 重点是 `:` 就是动态路由的绑定，绑定的形参的名字就是 `parms`

如 `/user/:username` 那么这个路由就是动态路由，形参是 `username`，如果匹配到了 `/user/zhangsan`，那么实际参数就是 `zhangsan`，一定要记住这个形参的名字，后面获取参数的时候会使用到。

## 获取动态路由的参数

使用\$router是跳转的路由，$route可以获取这个路由的参数。

$router就是拿到router的实例；挂载了一些方法

 `this.$route.params` 也就是在router对象中的routes路由映射规则中，可以直接获取到形参的名字，`params`就是形参的名字。



## 小结打包目录结构

![image-20220307083804188](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307083804188.png)

脚手架在打包整体的开发项目时，是将css和js单独抽离的。对于以前的单独webpack来说，是将css和js完全揉合在一块，如果项目非常的大，那么整体的打包文件就非常大，当用户在加载的时候浏览器就会出现短暂的空白期，这是不良好的体验，所以cli将css和js单独抽离处理，并且，对于js来说，总共分了三个js

- 第一个app.js是自己开发的项目代码，自己手动编写的业务代码
- 第二个manifest.js是整个项目的底层支撑，导入导出等语法，为了打包代码做的底层支撑。简单来说就是代码的打包功能。
- 第三个vendor.js是各种第三方的代码，比如vue-router/vue等等。

## 路由懒加载

![image-20220307085806409](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307085806409.png)

### 懒加载效果

![image-20220307085901566](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307085901566.png)

### 路由懒加载的写法

![image-20220307090405704](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307090405704.png)

这种写法也是懒加载的形式，用一个变量来保存对应的对象，这种写法的好处是统一组件管理。

![image-20220307090452078](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307090452078.png)

## 路由嵌套

注意路由嵌套步骤

- 先在路由indes.js中的父组件中定义子组件的对象，包括path和component
- 接着在父组件中，添加上router-view。这样才可以在父组件中显示子组件。
- 注意，在index.js路由设置中，使用children定义一个数组，然后数组中是各种子组件对象，并且子组件对象中的path不能添加 `/` 否则会出现问题，`/`表示从根开始索引，这是不正确的。子组件的根就是父组件，只要定义在了父组件，那么就可以直接使用，不要加 `/`

![image-20220307090813808](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307090813808.png)

### 路由嵌套中的转发

![image-20220307091842859](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307091842859.png)

对于一个大的组件，也可以设置路由转发。
```javascript
const routes = [
  { path: '/', redirect: '/home' },
  { 
    path: '/home', 
    component: Home,
    children: [
      { path: '', redirect: 'news' },
      { path: 'news', component: HomeNews }
    ]
  },
  { path: '/about', component: About },
  { path: '/user', component: User }
]
```
## 路由传递参数

两种方式传递参数

- 使用 params路径中的参数，适合少量数据
- 使用query，也就是get方式的传递参数，适合大量数据，可以传递对象

![image-20220307095457006](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307095457006.png)

```javascript
export default {
  name: 'App',
  data () {
    return {
      uid: 0
    }
  },
  methods: {
    homeClick() {
      this.$router.push('/home').catch(err => {})
    },
    aboutClick() {
      this.$router.push('/about').catch(err => {})
    },
    userClick() {
      this.$router.push('/user/'+this.id),
      this.$router.query(
        path='/',
        {
          name: zhangsan,
          age: 18
        }
      );
    }
  }
}
```

## router和route的区别

route 是当前路由，包含当前的路径，当前的参数等，router-link和router-view都是全局的组件，是通过router注册的全局组件。

router 是整个项目的路由管理器

简单来说学校，route就是学生，每个学生都在学校的范围内，route不等于router，router是管理所有路由的存在。



vue中所有的组件都是继承自vue的原型，也就是相当于最后的object，所有的类都继承了object，所以只要给vue原型添加了一个属性，或者一个方法，那么所有的组件都可以使用整个方法，都会有整个属性，相当于给object添加了一个属性，一个方法，所有继承object的类都会继承这个方法，这个属性。

vue的核心方法

![image-20220307101626017](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307101626017.png)

之所以每次使用$router都可以获取到同一个对象，是因为vue底层中做了相应的处理，只是返回一个对象，永远都是同一个对象。

而每次拿到的route是不同的对象，都是点击了以后活跃的路由，这也是在底层中做了相应的处理

![image-20220307102033110](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307102033110.png)

每次将活跃的路由赋值给$route$，每次取到的值就是点击后活跃的路由。

![image-20220307102214935](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307102214935.png)



## 导航守卫

导航守卫说白了就是一个钩子函数，在before之前怎么样，在after之后怎么样，都会进行回调，js最好用的地方就是回调，做一件事情之前用回调的方式告诉你，做完一件事情之后用回调告诉你

![image-20220307105429063](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307105429063.png)

导航守卫就是路由跳转过程中的一些钩子函数，再直白点，路由跳转是一个大的过程，这个过程可以细分为跳转前中后等等细小的过程，说的直白一点就是回调函数，类似于定时任务的调用，到达了什么就会回来调用函数。常见的声明周期函数，创建了组件实例后，挂载到dom上个后，页面发生更新以后。生命周期的八个回调函数，创建，挂载，更新，销毁。每个方法都有前后两个，所以一共是八个生命周期函数。

![image-20220307102752234](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307102752234.png)

导航守卫：若守卫不放行next，你就不能导航到下一个路由中去

原有的源代码中，页面的跳转三个函数/对象属性实现。在router生成中实现页面的跳转修改。router是所有路由的管理对象，实现了很多对于路由的方法。

![image-20220307105554830](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307105554830.png)

- to 目的地址，是路由 route对象
- from 原有位置 是路由对象route
- next 是一个函数，这个函数是页面跳转的关键，导航守卫如果不放行，或者是不调用这个而函数，那么页面就无法跳转，这就是导航守卫的作用。使用这个几个函数能够在页面条状时，便捷的修改一些数据，如页面的title。如果一个一个修改，那么实在是太麻烦了，但是在路由设计中，增加了一个元数据meta后，这个问题就很好的解决

![image-20220307105241221](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307105241221.png)

- - 首先在每个路由对象中，也就是routes中的route添加一个meta属性，然后meita属性里可以放title属性，
- 然后再页面跳转的过程中，去页面跳转的流程，也就是去 router.beforeEach(functionto, from, next{})方法中调用修改即可
- beforeEach就成为前置守卫 guard，其实就是一个钩子函数，就是一个回调函数，

```javascript
to， from都是路由对象 也就是在生成的router中的routes中的其中一个route，相当于是学校中的学生。next就是一个页面跳转函数。一定要注意区分router和route这两个对象。
router.beforeEach((to, from, next) => {
	document.title = to.meta.title
	如果修改不成功，那么是因为有嵌套子路由的存在，可以使用另外一个属性
	document.title = to.martch.meta.title
	是因为有嵌套路由的存在，导致获取不到组组件的meta数据
	next() 导航守卫放心next函数，否则无法跳转页面
})
```

- 如果觉得还需要调用next方法会有些麻烦，俺么可以使用后置钩子函数hook，后置钩子函数是指已经跳转完成了，那么就可以不调用next函数了。

```javascript
afterEach(function(to, from)) 其中的 to和from都是route对象，是一个路由对象

afterEach((to, from) => {
	document.title = to.martch.meta.title;
})
```

next函数其实可以做很多的事情，比如说判断用户是否登陆了，如果登录了可以访问相关的信息，但是如果没有登录的话，那么就会跳转到另外一个登录界面，使用的就是next函数进行跳转，next函数的参数不仅仅只是一个布尔值，还可以是一个路由对象，表示=跳转的方向。

## keep-alive和vue-router
keep-alive包裹住的子组件会多两个生命周期:activated和deactivated

如果没有keep-laive包裹，一旦切换就是销毁。

actived和deactive两个函数只有显示的区域被keep-alive包裹时才会生效。

默认情况下，如果页面中的组件开始了替换，那么原来的组件会被销毁，不会进入缓存中，为了用户体验，返回上一次的显示范围，此时应该阻止组件被销毁，保持原有的界面即可。其实就是生命周期的函数了。

![image-20220307111609435](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307111609435.png)

这里不要随便加空格，逗号后面直接写数字，因为是使用正则来描述的

![image-20220307112842315](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307112842315.png)



![image-20220307112955662](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307112955662.png)

## 阶段案例

使用vue创建页面。

![image-20220307124817182](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307124817182.png)

## 阶段案例总结

![msedge-20220307160814](Vue%E7%AC%94%E8%AE%B0.assets/msedge-20220307160814.gif)

这个案例很好的解释了，从路由定向，到动态绑定属性，到页面元素的切换和插槽的使用，已经父传子通信。

- 如果不熟悉模块化开发，可以先将整体的界面写出来，然后在抽取模块，将整体用一个一个的组件去代替，重点是掌握插槽的使用和父传子的通信，这两个基本上可以说是解耦的关键。
- 插槽可以形成一个一个的部件，而父传子则是将整体的往原有的整体上靠。
- 一定要注意插槽的使用，并且一定要做到父子解耦，做到模块能够重复利用。并不是说所有的界面都是解耦的，主页面的存在就是自己开发的耦合度比较高的一个，但是一个一个的组件是没有耦合关系的，只是在最后形成总体界面的时候需要将各个组件联系在一块，组件没有耦合，整体的界面只是搭积木一样搭起来的东西而已，只要组件之间好哟个，搭积木就是很快的事情，两三个标签，四五个属性即可。

在webpack.base.config.js下，进行别名的配置。

![image-20220307162208405](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307162208405.png)

给文件夹起起别名，这样使用的时候更加的方便。注意不能直接写后面的引用不能写@

![image-20220307162423738](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307162423738.png)

这样在项目中引用的时候直接使用定义的符号即可。无论是复制还是移动，文件的路径都不回错误。是哦也能够~进行引用。

![image-20220307162805405](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307162805405.png)

# ES6 Promise

## Promis简介

![image-20220307162916865](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307162916865.png)

## 场景

![image-20220307164016620](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307164016620.png)

模拟

![image-20220307164047775](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307164047775.png)

另外一种写法

![image-20220307170711941](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307170711941.png)

 ## 多个请求竟态

![image-20220307173130264](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307173130264.png)

## 常见的使用

Promise.all方法,只有等有所有的都完成了才会进行处理

# Vuex

![image-20220307174741179](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307174741179.png)

## 状态管理 state

![image-20220307175310742](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307175310742.png)

## 多界面状态管理

![image-20220307180919804](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307180919804.png)

## 共享状态

![image-20220307175738535](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307175738535.png)

## store 数据的仓库

理解router的生成和挂载时候的历程,就是store的历程。

store中有许多重要的对象或者属性或者方法，但是总的来说分类以下几个

- state 状态。 保存各种需要共享的变量
- mutations 操作。提供各种方法对保存的共享变量进行操作
- actions 行动。简单理解是只要有异步任务如网络请求等，都需要用到actions
- getters 获取。相当于计算属性的get方法，获取值，同时能够对值进行操作，比如说很多地方多需要用到state共享变量的平方，那么使用getters方法即可。
- modules 模块

![image-20220307183742584](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307183742584.png)

![image-20220307180619599](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307180619599.png)



使用的时候是提交

![image-20220307182841334](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307182841334.png) ![image-20220307182924630](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307182924630.png)

## store属性之state单一状态树

简单来说就是保存各种变量。修改的途径一定要经过mutations，mutations是存储同步方法的区域。

```javascript
const store = new Vuex.store({
  state:{
    counter:  0,
  }
})
```





![IMG-I3G-20220307_183427](Vue%E7%AC%94%E8%AE%B0.assets/IMG-I3G-20220307_183427.png)

## store属性之getters

如果共享的变量需要经过处理之后才能使用的，一般实在getters中处理以后再拿值。这样每个地方拿到的值都是经过处理的了，不用每个地方每个组件都处理一下数据。

```javascript
const store = new Vuex.store({
  getters:{
    power(state){ //默认就会有这个state参数
      return state.counter * state.counter
    }
  }
})
```

### 案例

![image-20220307184907372](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307184907372.png)

### 写法，可以有多个甚至还可以有自身的参数。

![image-20220307185540686](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307185540686.png)

### 返回一个函数，让调用者输入参数后返回结果

返回的不仅仅只是一个参数，甚至还可以返回一个函数，让其他的调用者输入参数后，就可以返回一个结果，这是优势，可以返回一个函数，让调用者输入参数后返回结果。

![image-20220307190129274](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307190129274.png)

![image-20220307190042147](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307190042147.png)



## store属性之mutations，actions

mutations中定义的各种函数就是一个时间，函数的名称就是事件类型。mutations的函数定义就分为两份，第一个是函数的名称就是时间的类型，所以使用commit进行提交时，第一个参数就是事件类型。

一定要记住在mutations中尽量不要写异步函数，否则devtools工具不能够跟踪到变化的数据。

![image-20220307190333535](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307190333535.png)

普通的使用方式是：

- 拿到store对象后，使用comit的方式进行提交，传入的参数是已经定好的事件类型，也就是再mutations中定义的函数。
- 第二种可以传递多个参数，从普通的组件中传递参数给mutations

![image-20220307190722027](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307190722027.png)

mutations

![image-20220307190742087](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307190742087.png)

### 传递复杂数据

和普通的数据传递类似，只不过数据类型变为了对象，复杂数据的传递直接使用对象类型进行传递。

![image-20220307191037049](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307191037049.png)

但是提交的参数最好只是一个，这个参数可以是一个复杂的数据。

### 提交风格

![image-20220307191456872](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307191456872.png)

对于第二种提交风格，会将自身作为一个对象传递过去，此时在mutations中的方法接受的参数就不是理想中传递的参数了，而是一个陈伟载荷的对象payload，如果需要取出数据，必须是对象的属性 也就是 playload.count，不再是一个单纯的参数，而是一个对象类型的载荷。

![image-20220307191709511](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307191709511.png)

###　响应式

Vue.set 和Vue.delete，能够做到响应式，最重要的是vue中已经添加到了响应式系统的变量才能够做到响应式，如果直接添加数据是做不到响应式的，需要用到Vue.set和Vue.delete两个方法才能做响应式。

![image-20220307193636366](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307193636366.png)

常量提交

![image-20220307194245660](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307194245660.png)

案例

![image-20220307194306567](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307194306567.png)

###　同步函数

![image-20220307211001632](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307211001632.png)

![image-20220307211149105](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307211149105.png)



在mutations中尽量不要使用异步函数，否则devtools不能很好的跟踪到变化的数据，这样导致bug难以寻找。如果真的需要异步函数，那么使用actions将mutations进行替代，相当于是异步函数使用的mutations。action异步执行完了在调用mutations改变state

![image-20220307194705070](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307194705070.png)

返回信息

在普通组件中提交异步的任务使用dispatch，dispatch就是给actions中的函数进行提交。actions中的函数处理数据是通过mutations的，只是异步函数多增加了这一个环节。

![image-20220307202114072](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307202114072.png)

actions的函数（事件，事件类型）

![image-20220307201759176](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307201759176.png)

在dispatch中传递信息过actions，然后actions中使用一个Promise对象对异步事件进行处理，然后返回事件是否成功给dispatch，dispatch中写then函数，即可得到actions中的信息。这是promise中很好的一个信息传递方式，在js中，对象是无穷力量的。

## store属性之 module

简单来说 module是为了解决单个store过于臃肿的问题，但是vue建议使用一个store，方便进行管理，此时就引申出module模块，module模块是一个对象数组，这个对象数组里面又可以定义store的五个属性，简单来说就是module模块就是一个小的store，能够通过总的store访问某一个模块，这个模块又是一个小的store，这个模块的声明可以分开进行编写，因为模块对应的就是一个对象。分开进行编写那么整体的结构也会比较清楚。

注意：模块数组modules中的各种对象是直接存放在总体的state中的，通过store可以直接进行引用。

> 模块中的各种属性就是政体中的一个，最后都会被合并的，只是写的位置不一样而已，所以在命名的时候记得不要重复命名函数和变量或者是getters的函数。只是模块中的函数可以有第三个参数，第三个参数是为了解决引用根store的问题设计的。第三个参数的名称就是rootStore。
>
> 但是模块都是有自己的作用范围的，比如说广东人和广西人，即使住在一块，但是还是有自己的省份的差异的。module中的函数比如commit和dispatch只能作用于自身模块中的函数，而不是作用其他模块或者根模块的函数

![IMG-I3G-20220307_205028](Vue%E7%AC%94%E8%AE%B0.assets/IMG-I3G-20220307_205028.png)



![image-20220307210848900](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307210848900.png)

## 目录结构

store中的五个属性都可以单独的抽出作为一个js文件，最后导出导入即可，值得蜘蛛的是module一般是建立一个文件夹modules，里面存放各种module，这样的结构不会凌乱，以后的开发中成员各自负责各自的module即可。

# 网络请求模块封装

## 网络请求模块

现在在前端的发送请求库中，当前axios是比较好用的发送请求框架。

![image-20220307215307560](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307215307560.png)

## 使用

注意axios是一个模块，不是插件，所以不需要使用vue.use全装axios。直接导入后然后加载配置文件即可开始使用.

axios支持了多种请求方式

### 常见的请求方式

![image-20220307223053049](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307223053049.png)

### 并发请求

![image-20220307223157045](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307223157045.png)

### 请求的各种定义

![image-20220307231843031](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307231843031.png)

注意params和data的区别，一个是get请求，一个是post请求，timeout超时时长是ms单位。

### 单独创建每一个实例

![image-20220307232948162](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307232948162.png)

在开发中，会出现不同的服务器地址。所以用改使用的是不同的地址，这时候用不同的axios实例就可以了，如果使用的是全局的axios明显不合适。

## 封装

封装统一接口，用第三方实现接口，以后改实现就行了，使用的接口都不需要改。

`axios.create({config配置文件})`即可生成axios对象，这个对象是局部的。

第一种封装方式

![image-20220307235419881](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307235419881.png)

第二种封装的方式

![image-20220307234931433](Vue%E7%AC%94%E8%AE%B0.assets/image-20220307234931433.png)

第三种封装方式

axios本身返回也是promise，可以直接return，但因为instance是axios的实例，直接return就没有封装的效果了，还是过于依赖axios。

![image-20220308000006205](Vue%E7%AC%94%E8%AE%B0.assets/image-20220308000006205.png)

第四种封装方式

利用axios返回的就是一个Promise，所以直接去掉newPromise即可

![image-20220308000333767](Vue%E7%AC%94%E8%AE%B0.assets/image-20220308000333767.png)

js中传递函数就是为了回调函数的使用。如在instance中，只要是外界的函数success，failure这两个就能够将axios的信息传递出去，这就是参数是函数的优势，能够快速的进行回调函数，也就是传递信息，回调信息。

函数作为参数传入其他函数，目的就是为了简化代码，同时也可以回调函数，进行数据交互。要想回调一个函数，最简单的方式就是将这个函数作为参数传递给调用者，或者使用Promise的方式调用函数。js的优势就在于这个不限定范围，一切都可以看作是对象，完完全全的全是对象，没有类型的严格限制，让功能实现更加的简单，但是再使用的时候容易懵，不知道这个参数具体是什么作用，还是一句话，多理解js的设计。



同时利用Promise的设计，可以将一个Promise进行返回，那么这个Promise就能够将数据处理和处理结果分开，如果将Promise对象返回，那么在处理结果时，可以在接受到promise对象出使用then或者catch

request就是对原有的函数的封装，最后request接收到的就是在封装axios过程中返回的Promise对象。这样就可以在request中调用then和catch方法。相当于一个人（promise）在美国买了一个手机（then），这个人（promise）将手机（then）带回了中国（request），这样在中国的promise也可以使用这个手机（then），这个就是函数封装的思想，一个函数可以直接调用，也可以在另外的地方进行调用。

![image-20220308072828176](Vue%E7%AC%94%E8%AE%B0.assets/image-20220308072828176.png)

>  1.把函数作为参数传过去 2.传过去之后反过来回调这个函数

## 拦截器

提供了四种拦截器

使用的方式都是：请求和响应，每一个都分别有一个是成功的时候的回调，要给是失败的时候的回调。如果需要在外部处理信息，那么 四个函数都必须要将信息return出去，否则拿不到数据。

```javascript
请求
axios.interceptors.request.use( config=>{
 成功的回调，传递的参数是配置，必须要return出去
},err=>{
  失败时候的回调，传递的参数是失败的错误信息
})


axios.interceptors.response.use(res=>{
  成功响应时的回调，传递的参数时响应的信息。必须要返回结果，可以直接返回data
  return res.data
},err=>{
  响应失败时候的回调，传递的参数是响应失败时的信息。
})
```

- 请求成功
- 请求失败
- 响应成功
- 响应失败

拦截器拦截了东西，在处理完成了之后必须要return 出去，否则axios不能使用被拦截的东西，必须要return出去。

- 常见的使用是检查config的配置是否符合要求，或者是单独的给某一个请求进行配置
- 或者是在请求的过程中可以添加一些动画的效果，就是发送请求的时候调用一下动画的函数，即可达到一点发送请求就有网络动画的出现。
- 有些网络界面必须要进行登录，对于前后端分离的项目来说，由前端进行判断，而不再是后端了，这样会减少服务器的压力
- 前端只能做基本的拦截和判断，减轻一下服务器的压力，但是如果是恶意攻击，那么还是只有后端的拦截才有用处，因为恶意攻击不通过前端的流程了，直接用脚本刷

![image-20220308074018440](Vue%E7%AC%94%E8%AE%B0.assets/image-20220308074018440.png)



# 项目练习

1. 目录结构划分

   在src文件夹中

    - assets资源
        - images图片
        - css文件
        - js文件

    - components存放公共的组件
        - 里面可以分为两个文件夹，一个是所有项目都可以复用的common
        - 一个是只在当前项目使用的content
		- views 视图文件夹，存放各种组件组合成的视图
		- router 路由文件夹
		- store 数据仓库文件夹
		- network 网络请求文件夹

在总体文件夹中，用到了一部分公共的文件和工具类 文件夹 common

## 别名

![image-20220308121337869](Vue%E7%AC%94%E8%AE%B0.assets/image-20220308121337869.png)

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  configureWebpack: {
    // 解决路径相关的问
    resolve: {
      extensions: [],//解决的省略后缀
      alias: { // 解决别名问题
        'assets': '@/assets',
        'common': '@/common',
        'network': '@/network',
        'views': '@views',
      }
    }
  }
})

```

## 封装

无论是什么时候都需要用到封装，都需要进行解耦和，记住一点，只要是未来可能会修改的东西，都需要解耦合，比如说 ip服务器地址，不能让ip服务器地址出现在逻辑处理中，而应该封装一个函数能够直接调用这个函数得到结果，燃火在进行逻辑处理，封装一个只会发请求得到结果的函数，逻辑处理的这个部分一般来说不会变化，变化的大可能是请求地址，所以封装了一个能够单独发送某一个请求的函数就可以了，逻辑处理分开另外一个函数进行，这样即使逻辑处理也需要修改，那也不会改动请求函数这一部分，这就是好处，也就是一个函数只做一件事情。

一个大的组件最好封装一个单独的js文件为这个大组件中的各种小组件进行服务，这个js文件中的各种函数就是这个大组件中的各各种小组件的数据。把这个函数想象成能够获得数据的地方就可以了，至于怎么获得数据是这个函数的问题，组件只需要进行调用函数即可。

## 请求

组件中都是活的数据，所以数据被封装的函数中来，那么数据该什么时候开始获得呢？也就是什么时候开始调用函数？别忘了，vue给我们提供了八大生命周期的函数，一般组件创建成功了就开始请求数据即可，也就是created回调函数中进行调用获取数据的函数。 在js的代码中，如果能够经常使用Promise对象，是能够很好的优化我们的代码的，最主要的是then函数能够给我们意想不到的惊喜，就是尝试用Promise对象，那么就会有then方法，Promise对象在完成了异步的事情之后，会自动调用then方法，所以无论对象在哪里，都可以自动then方法。

## 数据存储

![image-20220308181942014](Vue%E7%AC%94%E8%AE%B0.assets/image-20220308181942014.png)

不能使用.type，点运算符后面跟的是字符串类型的属性名，但是这里的type是变量，需要经过计算，只能使用中括号[type]。注意有些时候不能直接用点运算符好，只能使用中括号，因为点运算符号只能得到字符串类型的key的value，如果是对象中的对象，或则对象中的数据，一定要注意使用中括号。变量形式的要用[]

- 对于已经存在的属性和方法，用.和用[]得到的结果一致
- 对于不存在(未定义)的属性和方法，用.会创建这个新的属性或方法，而用[]的方式访问不会创建新的属性或方法
- []里面可以传变量、参数、表达式，而.后面只能跟一个固定的属性

## 移动端的滑动属性

[better-scroll/README_zh-CN.md at master · ustbhuangyi/better-scroll (github.com)](https://github.com/ustbhuangyi/better-scroll/blob/master/README_zh-CN.md)

移动端的滑动效果尽量使用一些框架带有的滑动效果。

目前最多使用的是： betterscroll，生成对象，这个对象中需要两个参数，第一个是需要被滑动的对象，一个容器中一般只含有一个元素因为betterscroll只对第一个元素有效果。

1：非实时（屏幕滑动超过一定时间后）派发scroll 事件；2：在屏幕滑动的过程中实时派发 scroll 事件；3：不仅在屏幕滑动的过程中，而且在滚动动画运行过程中实时派发 scroll 事件。

![image-20220309070645209](Vue%E7%AC%94%E8%AE%B0.assets/image-20220309070645209.png)

监听滑动和上拉加载更多

![image-20220309072223713](Vue%E7%AC%94%E8%AE%B0.assets/image-20220309072223713.png)



总结

组件不能添加监听，只有加上修饰符 .native修饰符之后才可以进行监听。当我们需要监听原生的时间时，需要给绑定的事件添加上.native属性，否则无法进行监听事件。当然 子传父除外，这个事件能够不加上,native也可以进行监听。如果需要监听子组件，那么直接在父组件中直接监听标签即可，使用.native修饰符好进行即可。

### 事件总线
各种组件之间的互相联系，即非父子通信下的通信
```js
发送事件
this.$bus.$emit('事件类型')

接受 监听 事件
this.$bus.$on('事件类型')
```



![image-20220310000859316](Vue%E7%AC%94%E8%AE%B0.assets/image-20220310000859316.png)



![image-20220310000814539](Vue%E7%AC%94%E8%AE%B0.assets/image-20220310000814539.png)
简单来说，就是利用vue的原型，也就是Java中的object，一切对象的基础，这样就能够使用事件总线这个对象了。



防抖和节流

通常会有这样的需求，在input输入框中，每次用户输入完成之后都会向服务器发送请求，显示出推荐的搜索关键字，但是没有必要每次用户输入一个字符就发送一次请求，这样服务器的压力也大。所以应该设置节流函数，也就是固定多少事件发送一次请求，无论用户输入的是多少字符，在固定事件都是之发送一次请求。节流：在规定的时间内只执行一次。防抖：Debounce 节流：throttle

节流阀就是在定时器为生效前就取消这个定时器，节流阀每次调用都会先清除上一个定时器，最后只剩下一个定时器

![image-20220310103915855](Vue%E7%AC%94%E8%AE%B0.assets/image-20220310103915855.png)

简单的来说就是每次设置延时，但是这个延时的函数还没有到调用的时间，比如说设置了500ms后进行调用，但是其他的函数在300ms时就调用了防抖动函数，那么会将这个定时器清除掉，也就是之前的还剩下200ms就可以执行的函数不执行了，新的定时器又重新设置了500ms，循环重复，只要防抖动函数被调用就能重复这个过程，这样即使其他的函数频繁的调用防抖动函数，但是对于需要执行的函数，如果被调用的很快，那么定时器每次都会被顶掉，那么需要执行的函数就不会执行。只有等到了定时器开始执行了，才会被调用这个需要执行的函数。防抖动就是利用了定时器的还没有来得及开始就已经结束，已经被替换的思想，这样每次都会被延后，总体的调用次数就降低了。 



获取组件对应的元素，使用的是 \$el这个属性，每个组件的对象可以先使用refs这个属性获得，然后使用\$el这个属性就能拿到这个组件对应的标签元素。

tabControl中sticky失效的问题

使用better-scroll提供的position进行动态的修改，或者是使用HTML元素的offset top的属性值进行修改。



更巧妙的是使用两个navbar，第一个bar布局在头部的正上方，然后先进行隐藏，然后当第二个navbar到达和第一个bar相同位置的时候进行隐藏即可。当第二个bar又开始往下滑露出来的时候将第一个bar隐藏，这样做出来的动画效果就是合适的了。



```js
    scroll(position) {
      // console.log(position);
      if (position.y <= -645) {
        this.$refs.tabControl.$el.style =
          "position: fixed; top:" + Math.abs(position.y + 10) + "px;";
        this.$refs.popular.$el.style = "height: 315px;";
        console.log(position.y);
      } else {
        this.$refs.tabControl.$el.style = "position: static;";
        this.$refs.popular.$el.style = "height: 275px;";
      }
    },
```



## 防抖和节流的区别

防抖就是在调用函数的时候如果频繁的调用那么就使用定时器，这种效果进行推研，如果是节流，那么就是在发送信息的时候不发送那么多，只发送有代表新的信息即可。说白了就是防抖就是在调用别的函数的时候尽量少的调用，节流就是发送信息给其他函数的时候少发送。



## 大量数据vue渲染很慢

尽量不使用计算属性进行更新，最好是这个组件直接拿到数据。



## 动态路由

每次拿到的id是不一样的(url可见)，一样的是页面显示的(因为keep-alive页面没有重新创建,也就没有重新从$route中取出参数)

获取id一样的在activated里面拿iid

在路由规则中设置 path: 'detail/:iid'



## 封装复杂类型的数据

```js
export class Goods {
 constructor(itemInfo, columns, services) {
   this.title = itemInfo.title
   this.desc = itemInfo.desc
   this.newPrice = itemInfo.newPrice
   this.oldPrice = itemInfo.oldPrice
   this.discount = itemInfo.discount
   this.realPrice = itemInfo.lowNowPrice

   this.columns = columns
   this.services = services
 }
}
```

当某一个组件使用的数据较为复杂时，使用面向对象的封装才是最合适的，方便修改和方便调试

![image-20220311130932635](Vue%E7%AC%94%E8%AE%B0.assets/image-20220311130932635.png)



![image-20220311131646460](Vue%E7%AC%94%E8%AE%B0.assets/image-20220311131646460.png)



![image-20220311132009857](Vue%E7%AC%94%E8%AE%B0.assets/image-20220311132009857.png)



![image-20220311154739434](Vue%E7%AC%94%E8%AE%B0.assets/image-20220311154739434.png)



![image-20220311155055513](Vue%E7%AC%94%E8%AE%B0.assets/image-20220311155055513.png)



在计算高度的过程中，对于图片的处理方式有许多种，第一种可以给图片添加对外发射信号，表明当前的图片已经加载完成了，第二种是使用一个函数进行防抖的操作，在发射时减少发射的次数是节流，在调用时减少调用的次数是防抖，这两个方式对于提高性能是非常重要的。

![image-20220311160057439](Vue%E7%AC%94%E8%AE%B0.assets/image-20220311160057439.png)



电商平台开发中，一定要注意首页模块的商品和推荐模块商品之间的联系这样能够降低开发的难度。

mutation的作用是单一的，只是负责数据的修改，其余的判断等操作放在actions里面，这样既避免了异步事情，同时将mutations中的事件简化了，素有的mutations只是修改数据，而不是用来判断逻辑的。

![image-20220312153403428](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312153403428.png)

一切都是为了之后如果做大型项目时，能够快速的跟踪错误的存在。



全选按钮的高性能方案：用一个变量记录选中的长度和总体的长度，需要比较时，直接对比两个数据就可以了，不需要遍历全部的数据，记得用一个变量对选中的长度和总体的长度进行比较。



# Vue插件封装

第一个写出需要被封装的组件原型

1. 生成组件构造器
2. 利用组件构造器生成组件对象
3. 利用js原生方法创建一个元素
4. 利用\$monted函数将组件对象挂载在元素对象上
5. 最后利用原型 protoType 将组件对象传给vue对象，因为vue中所有的对象都是继承了vue，所以vue有的子元素都有。

vue使用插件时，会默认调用插件中的install方法，所以封装差价必须要设置install 方法，否则不会生效的。

![image-20220312172048083](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312172048083.png)

和vue.components中不一样的是，直接使用\$mount是可以直接讲组件挂载到某一个元素上的，vue.components是一个注册组件的方式，注册组件后还需要在父组件中的components中进行使用子组件，而插件是直接挂载到页面中的一个元素，平时不显示，需要用到是调方法显示即可。

### 主页面

![image-20220312172354236](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312172354236.png)

### 分类页面

![image-20220312172426271](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312172426271.png)

### 购物车页面

![image-20220312172554259](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312172554259.png)



### 我的页面



![image-20220312172501885](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312172501885.png)

### 减少移动端300ms的问题 fastclick





# 响应式原理

理解vue的双向绑定原理，也就是响应式原理，不是页面的响应式而是数据的响应式对于理解和使用vue是一个很大的帮助。

message定义在data里被touch到时，会生成getter和setter方法，然后使用到message的地方会对应生成watcher，被放到dep容器里。

当message发生改变时，会遍历通知message对应dep里的所有watcher，去调用他们的update方法进行更新

![image-20220312181104549](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312181104549.png)



![image-20220312181503086](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312181503086.png)

![image-20220312194736798](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312194736798.png)



vu内部通过defineProperty方法，对一个对象的属性（key）重新进行定义，对于一个key，都加入了set和get两个函数，这两个函数内进行了其他函数的封装，就能够达到监听的效果，对vue中某个对象的属性进行更改时，能够得到对应的数据，这个不是轮询做的，而是对于某一个属性的修改时，修改属性的时候顺便执行了第三方的方法，这个第三方的方法是通过defineProperty方法进行设置的，对于一个对象的属性重新定义，在定义这个对象的新属性的指向时，这个属性不仅仅有存储取出原有属性的值了，而且顺便会执行其他的函数，通过set和get方法来获取值，那么在gei和set内又可以进行其他的操作，调用其他的函数，这就是观察者模式，某一个数据改变了，执行了第三方的方法，通知了第三方，然后vue总线就会检查引用这个数据的所有观察者，通知其进行修改，调用对象的set函数直接进行修改，（这里的实现注意回环，一定要注意回环。）这样基本上就可以是实现vue的双向绑定了。

最重要的也就是最起初的函数 `Object.defineProperty(obj, key,value) == Object.defineProperty(obj, key, {function()})` 能够把属性对应饿变为了一个函数，这个也是js的优势，因为函数就可以是一个执行功能的函数，同时又能够转换为对象的含义



通知虽然只是两个字，但是怎么进行通知呢？怎么获取谁引用了这个属性的对象呢？答案是发布订阅这模式。

具体的思路就是观察者模式，不清楚的可以参考，主要就是发布者调用方法对一个存有各种监视者的数组进行遍历取出对象分别调用更新方法

这个在现在严格来说应该是叫观察者模式，发布订阅一般指解耦观察者与订阅者的模式

发布者（依赖 dependence）一般一个对象的每一个属性都对应的是一个dep，方便管理，每个dep对象中都存放的是订阅者的信息，也就是subs数组

![image-20220312205525593](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312205525593.png)



订阅者（观察者 watch）vue在发现某些对象类似mustache语法，引用数据时，就会将这个mustache语法对象和一个watcher进行绑定，然后将这个watcher放入对应的发布者中，这个发布者加入的subs数组就是mustache引用的数据的那个对象，这样当数据发生变化时，就能够通知到watcher，然后因为watcher和对应的mustache对象进行绑定，那么数据更改时也就能够通知到mustache即使进行更改了。

![image-20220312205601781](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312205601781.png)



```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>发布订阅模式</title>
</head>
<body>
  <div id="app">
    <h2> {{name}}</h2>
  </div>
  <script src="../../../node_modules/vue/dist/vue.js"></script>
  <script>

    const app = new Vue({
      el: '#app',
      data: {
        name: 'zhangsan'
      },
    })

  // 对于一个对象，其属性能够被重构
  const obj = {name:'list',age:18}

  // 1. 对于原有的对象的属性进行重构，以便于监听，但是不影响原有的功能。
  /**
   * 对于对象的每一个属性进行代理
   * Object.defineProperty(对象，对象属性，属性值)，这里的属性带上引号
   * 进行代理前，应该将原有的value进行保存，属性重新赋值自动调用set方法
   * 直接查找对象是找不到对应的属性的，因为是经过了代理，
   * 必须要指定属性了，才能够获取值，
   * 同时也不要写成了 obj[key] = newValue 或者是 return obj[key]
   * 否则会引起回环，网络回环
   */
  Object.keys(obj).forEach(key => {  // 取出所有的key进行迭代
    
    let value = obj[key] // 存储原有的value
    
    Object.defineProperty(obj, key, {  // 代理所有的key

      set(newValue) {
        console.log("监听 "+ key +" 值发生改变");
        value = newValue
      },

      get() {
        console.log("监听 " +key+ " 的值被获取");
        return value
      }
    })
  })

  // 观察者
  class Watch{
    constructor(name) { // 观察的名字
      this.name = name
    }

    update() {
      console.log("当前对象发生了变化");
    }
  }

  class Dep {
    constructor() { // 发布者储存订阅者的信息
      this.subs = []
    }

    addSub(watch) { // 发布者添加订阅者的信息
      this.subs.push(watch)
    }

    notify() { // 对订阅者发送消息，表明观察的变量更新了
      this.subs.forEach(sub => {
        sub.update()
      })
    }
  }

  // 订阅===中介
  let w1 = new Watch('zhangsan') // 服务于张三的订阅
  let w2 = new Watch('lisi') // 服务于李四的订阅
  let w3 = new Watch('wangwu') // 服务于王五的订阅

  // 仅仅有订阅者是不行的，发布者还需要将各种订阅者的信息存储起来
  let dep = new Dep()
  dep.addSub(w1)
  dep.addSub(w2)
  dep.addSub(w3)


  dep.notify() // 当发布者需要通知各个订阅者时，直接调用通知这个函数即可

  </script>
</body>
</html>
```

响应式原理

![image-20220312212216048](Vue%E7%AC%94%E8%AE%B0.assets/image-20220312212216048.png)



文档碎片 优化dom操作性能



因为每次都会移除，所以移除以后下一个节点就变成firstChild，while就可以一直循环直到没有child



这里这个Dep.target 是Dep的静态属性  不是实例对象属性 好多人估计这里懵了

初步理解的话不需要关注Compile内部实现 只需要知道Watcher对象是在Compile内部创建的就行 主要看observer和watcher的构造函数理清思路便可