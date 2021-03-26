# vue

## 1. 安装

crm学习法， 安装学习vue

按照ppt来安装配置项目

官网目录扫描一眼， 以后可能用的到

**el-input:**

* ```html
  <el-input v-model="listQuery.title" placeholder="Title" />
  ```

  这里的v-model 就是将input的值和`listQuery.title `进行绑定, 当input里面的值变化会改变`listQuery.title `

* ```html
  <el-input v-model="listQuery.title"  @keyup.enter.native="handleFilter" />
  ```

  这里的`@keyup.enter.native`:回车触发事件:`handleFilter`

* ```html
        <el-select v-model="listQuery.importance" placeholder="Imp" clearable >
    
  ```

  这里的`clearable`意思是是否可以清空选项

* ```html
          <el-option v-for="item in importanceOptions" :key="item" :label="item" :value="item" />
  ```

  这个循环的写法: 另外**key**的作用, **防止跳跃删除**

## 2. vue实例

封装数据n, 以及相关所有操作

![image-20210324154720130](https://i.loli.net/2021/03/24/OybqzpnGHle36Kr.png)

****

### 2.1 options相关概念

选项. 是vue构造函数生成实例的参数.

#### 2.1.1 options包括

* 数据：data、props、propsData、computed、methods , watch
* DOM
  * el: 挂载点
  * template: html, 不同在于有些占位符, 完整用
  * render: 和template相互替换, 非完整版用
* 生命钩子
  * ***什么是钩子?***: 就是在某一点执行的特殊函数. **钩子**按照词意就是具**有切入点的东西**, 而生命周期函数就是在  **不同切入点**执行的函数
* 资源:directives、filters、components
* 组合
* 其他: 先不看

#### 2.1.2 options学习的重点有哪些?

![image-20210324162254890](https://i.loli.net/2021/03/24/cSIQZMVqByx4b83.png)

![image-20210324162307186](https://i.loli.net/2021/03/24/UafpRvirhYmLkbD.png)

****

### 2.2 options开始学习

1. `el`: main.js如下:

   ```js
   import Vue from 'vue'
   console.log(window.Vue)
   Vue.config.productionTip = false
   import Demo from './Demo.vue'
   new Vue(
   	el:"#app",
       render:h=>h(Demo)
   )
   ```

   过程: `index.html`会加载`main.js`, 然后用`Demo.vue`中的内容替换`index.html`中的`#app`节点

   还有一种写法:

   ```js
   new Vue(
       render:h=>h(Demo)
   ).$mount("#app")
   ```

2. `data`: 可以是个对象也可以是个返回对象的函数, **推荐写成函数的形式**

   ```js
   new Vue(
      data(){
       return{
         n:0
       }
      },
      template:`
   	<div>
   	{{n}}
   	<button @click="add">+1</button>
   </div>
   `    
   ).$mount("#app")
   ```

   ***为啥推荐写成函数形式?***

   当不同组件引用这个数据的时候, 如果是data是对象, **不同组件用的是同一个对象**, 修改会造成混乱. 如果是函数, **不同组件用的是, 不同数据对象**.

3. `methods`: 函数集合, 包括两类:

   * **事件处理函数**:

     ```js
     new Vue(
        data(){
         return{
           n:0
         }
        },
        template:`
     	<div>
     	{{n}}
     	<button @click="add">+1</button>
     </div>
     `,
       methods:{
           add(){
               this.n += 1
           }
       }
     ).$mount("#app")
     ```

     **注意:** 以后自己一定会出现的bug, 把`add()`函数**写在methods**外面, 一定会报

     > ReferenceError: add is not defined

     这样的错误

   * **普通函数**: 写在组件里面

     ```js
     new Vue(
        data(){
         return{
           n:0, 
           array: [1, 2, 3, 4]
         }
        },
        template:`
     	<div>
     	{{n}}
     	<button @click="add">+1</button>
     	<hr>
     	{{filter()}}
     </div>
     `,
       methods:{
           add(){
               this.n += 1
           },
           filter(){
               return this.array.filter(i=>i%2===0)
           }    
       }
     ).$mount("#app")
     ```

     问题在于:`template`重新渲染, **就会重新执行普通函数**

     

4. `components`: 可以被复用的vue文件. 

   ***vue组件和vue实例有什么不同?***

   vue实例是**由`new Vue()`创建出来的**, vue实例**不包含挂载点**

   实例变组件: 

   1. 复制实例中的options
   2. 组件中新建`template`, `script`,`style`, 标签
   3. 将复制的内容放到组件的`script`中
   4. 将`script`中的, template内容剪切到`template`标签中

   **如何使用vue组件?**

   ```js
   + import Demo from './Demo.vue'
   new Vue(
   +    components:{
   +    	Frank:Demo
   +    },
      data(){
       return{
         n:0, 
         array: [1, 2, 3, 4]
       }
      },
      template:`
   	<div>
   	{{n}}
   	<button @click="add">+1</button>
   	<hr>
   +   <Frank/>
   	{{filter()}}
   </div>
   `,
     methods:{
         add(){
             this.n += 1
         },
         filter(){
             return this.array.filter(i=>i%2===0)
         }    
     }
   ).$mount("#app")
   ```

   第二种方法:

   ```js
   + Vue.component("Demo2", {
   +    template:`
   +	<div>demo222</div>
   + `
   + })
   
   new Vue(
      data(){
       return{
         n:0, 
         array: [1, 2, 3, 4]
       }
      },
      template:`
   	<div>
   	{{n}}
   	<button @click="add">+1</button>
   	<hr>
   +   <Demo2/>
   	{{filter()}}
   </div>
   `,
     methods:{
         add(){
             this.n += 1
         },
         filter(){
             return this.array.filter(i=>i%2===0)
         }    
     }
   ```

   第三种方法:

   ```js
   new Vue(
   +    components:{
   +      Frank:{
   +    	data(){
   +    		return{n:0}
   +		},
   +        template:`
   +			<div>Frank's n: {{n}}</div>
   + `    
   +   }
   +    },
      data(){
       return{
         n:0, 
         array: [1, 2, 3, 4]
       }
      },
      template:`
   	<div>
   	{{n}}
   	<button @click="add">+1</button>
   	<hr>
   +   <Demo2/>
   	{{filter()}}
   </div>
   `,
     methods:{
         add(){
             this.n += 1
         },
         filter(){
             return this.array.filter(i=>i%2===0)
         }    
     }
   ```

   

5. `生命周期函数`:直接写在options的第一层

   * create: 出现内内存中, 不在页面中
   * mounted: 出现在页面中
   * destroyed: 从页面中消亡

6. `props`: 接收外部属性

   **加冒号代表后面的是代码**

   第一行传的是**字符n**, 第二行传的是**变量n**, 第三行传的是**函数add**

   ```js
   tmplate:`
   	<div>
   		<Demo message="n" />
   		<Demo :message="n"/>
   		<Demo :fn="add"/>
   	</div>
   `,
   methods:{
       add(){},
       toggle(){
           this.visible = {this.visible}
       }
   }    
   ```

   接收参数:

   ```html
   <template>
   	<div class="red">
       	<button @click="fn">
               call fn
           </button>
           </button>
       </div>
   </template>
   <script>
   	export default{
           props:{"message", "fn"}
       }
   </script>
   ```

## 3. 数据响应式

### 3.1 get 和 set

**情况一:** 当直接修改自定义的外部数据依旧能触发对页面的重新渲染

```js
const myData = {
  n: 0
}
// console.log(myData) // 本节课精髓

new Vue({
  data: myData,
  template: `
    <div>{{n}}</div>
  `
}).$mount("#app");

setTimeout(()=>{
  myData.n += 10
  // console.log(myData) // 本节课精髓
},3000)
```

> 3秒之后, 页面从0变成了n

**情况二:**3秒后log同一个数据,结果完全不同

```js
const myData = {
  n: 0
}
 console.log(myData) // 第一次log

new Vue({
  data: myData,
  template: `
    <div>{{n}}</div>
  `
}).$mount("#app");

setTimeout(()=>{
  myData.n += 10
   console.log(myData) // 第二次log
},3000)
```

> 结果如下:

![image-20210325170624411](https://i.loli.net/2021/03/25/u9o3wpa6gVmkvD4.png)

***

***什么是get?***

为了不加括号执行一个函数

```js
let obj0 = {
  姓: "高",
  名: "圆圆",
  age: 18
};

// 需求一，得到姓名

let obj1 = {
  姓: "高",
  名: "圆圆",
  姓名() {
    return this.姓 + this.名;
  },
  age: 18
};

console.log("需求一：" + obj1.姓名());
// 姓名后面的括号能删掉吗？不能，因为它是函数
// 怎么去掉括号？

// 需求二，姓名不要括号也能得出值

let obj2 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  age: 18
};

console.log("需求二：" + obj2.姓名);
```

> "姓名" 就称为计算属性, 经过计算才能得到的值

***

***什么是set?***

不加括号就修改类的属性, 用**类似于属性赋值的方法**

```js
// 需求三：姓名可以被写

let obj3 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  set 姓名(xxx){
    this.姓 = xxx[0]
    this.名 = xxx.slice(1)
  },
  age: 18
};

obj3.姓名 = '高媛媛'

console.log(`需求三：姓 ${obj3.姓}，名 ${obj3.名}`)

// 总结：setter 就是这样用的。用 = xxx 触发 set 函数
```

***

***最后打印object3, 会发生什么?***

![image-20210325173720925](https://i.loli.net/2021/03/25/ZvTnXYJiCBodjWO.png)



>  "姓"和"名"都是正常属性, 而"姓名"被`(...)`代替

因为"姓名", 不是正常的属性, 而是被`get`,`set`改造过的属性 

### 3.2 Obeject.defineProperty

***如何在对象定义结束之后再加上虚拟属性?***

```js
var _xx = 0
Object.defineProperty(obj3, 'xxx',{
    get(){
        return _xx
    },
    set(value){
        _xxx = value
    }
})
```

### 3.3 代理和监听

***

***需求一: 如何做到在对象定义之后加上虚拟属性?***

```js
// 需求一：用 Object.defineProperty 定义 n
let data1 = {}

Object.defineProperty(data1, 'n', {
  value: 0
})

console.log(`需求一：${data1.n}`)
```

***

***需求二: 监控属性n的写入, 当写入的值小于0就无效***

在`set`里面进行监控

```js
let data2 = {}

data2._n = 0 // _n 用来偷偷存储 n 的值

Object.defineProperty(data2, 'n', {
  get(){
    return this._n
  },
  set(value){
    if(value < 0) return
    this._n = value
  }
})

console.log(`需求二：${data2.n}`)
data2.n = -1
console.log(`需求二：${data2.n} 设置为 -1 失败`)
data2.n = 1
console.log(`需求二：${data2.n} 设置为 1 成功`)
```

***

***需求三: 如何在上面的需求上不暴露_n?***

```js
// 需求三：使用代理

let data3 = proxy({ data:{n:0} }) // 括号里是匿名对象，无法访问

function proxy({data}/* 解构赋值，别TM老问 */){
  const obj = {}
  // 这里的 'n' 写死了，理论上应该遍历 data 的所有 key，这里做了简化
  // 因为我怕你们看不懂
  Object.defineProperty(obj, 'n', { 
    get(){
      return data.n
    },
    set(value){
      if(value<0)return
      data.n = value
    }
  })
  return obj // obj 就是代理
}

// data3 就是 obj
console.log(`需求三：${data3.n}`)
data3.n = -1
console.log(`需求三：${data3.n}，设置为 -1 失败`)
data3.n = 1
console.log(`需求三：${data3.n}，设置为 1 成功`)
```

***

***需求四, 五: 如何在上面的基础上,如果括号里面传入的是有名对象?***

```js
// 需求五：就算用户擅自修改 myData，也要拦截他

let myData5 = {n:0}
let data5 = proxy2({ data:myData5 }) // 括号里是匿名对象，无法访问

function proxy2({data}/* 解构赋值，别TM老问 */){
  // 这里的 'n' 写死了，理论上应该遍历 data 的所有 key，这里做了简化
  // 因为我怕你们看不懂
  let value = data.n //这里保存了n值, 但是之后的会新生成虚拟属性n, 会代替这个n值
  Object.defineProperty(data, 'n', {
    get(){
      return value
    },
    set(newValue){
      if(newValue<0)return
      value = newValue
    }
  })
  // 就加了上面几句，这几句话会监听 data

  const obj = {}
  Object.defineProperty(obj, 'n', {
    get(){
      return data.n
    },
    set(value){
      if(value<0)return//这句话多余了
      data.n = value
    }
  })
  
  return obj // obj 就是代理
}

// data3 就是 obj
console.log(`需求五：${data5.n}`)
myData5.n = -1
console.log(`需求五：${data5.n}，设置为 -1 失败了`)
myData5.n = 1
console.log(`需求五：${data5.n}，设置为 1 成功了`)
```

**小结**

**·Object.defineProperty**

* 可以给对象添加属性value
* 可以给对象添加getter/setter
* getter/setter用于对属性的读写进行监控

**啥是代理（设计模式）**

* 对myData对象的属性读写，全权由另一个对象vm负责
  那么vm就是myData的代理（类比房东租房）
  比如myData.n不用，偏要用vm.n来操作myData.n

**vm=new Vue({data:myData})**

* 一、会让vm成为myData的代理（proxy)
* 二、会对myData的所有属性进行监控
* 为什么要监控，为了防止myData的属性变了，vm不知道
  vm知道了又如何？知道属性变了就可以调用render(data)呀！
  Ul=render(data)