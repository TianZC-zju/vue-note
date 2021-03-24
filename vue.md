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

