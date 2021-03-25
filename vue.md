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

***options是什么?***

选项. 是vue构造函数生成实例的参数.

***options包含哪些?***

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

***options学习的重点有哪些?***

![image-20210324162254890](https://i.loli.net/2021/03/24/cSIQZMVqByx4b83.png)

![image-20210324162307186](https://i.loli.net/2021/03/24/UafpRvirhYmLkbD.png)

****

***options开始学习:***

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

   