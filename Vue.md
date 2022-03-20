## 基础
- 导入

    ```js
    //生产环境
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    ```

- MVVM模型：M是data中的数据、V是容器、VM是vue实例对象

- vue管理的函数必须写成普通函数，反之则可以写成箭头函数(ajax、timer、promise等)

- 定义vue容器、属性时单个单词用`Name`、多个单词用'`name-bing'`；文件名用驼峰命名

## 渲染页面

- 模板

    ```js
    <div id='test'>                 //容器，一个vue实例对应一个html容器
        <h1>{{name}}</h1>           //插值，内可以是js的语句、变量、表达式；vue实例的数据、函数等
    </div>
    
    <script type='text/script'> 
    	const vm = new Vue({       
        	el: '#test',
        	data: {
            	name: 'wang',
        	}
    	})
    </script>
    ```

- el挂载的容器

    ```js
    vm.$mount('#test');         //全局挂载，通常链式跟随在最后
    ```

- data

    > `vm_data.name === vm$data.name === vm.name`

    ```js
    data() {              //函数式(组件必用)
        return {
            name: 'wang'
        }
    }
    -------------------------------------------
    data: {               //容器获取data第一层属性时必须初始化|在computed创建；获取第二层属性时可不必初始化(默认返回undefined，但undefined不被页面渲染)
        sex: {
            man: '男',
            men: '女',
        }
    }
    ```

    





```js
<student></student>                           //使用组件(可复用多次)，脚手架可用单标签

const student = Vue.extend({code;})           //创建组件，template属性为必须(不能创建el)、data必须为函数式
const student = {
    name: 'Student',                          //定义标签名(与组件名一致)
    template: `<p>{{name}}</p>`,
    data() {
        return {
            name: 'wang'
        }
    },
}
Vue.component('student',student);              //全局注册组件
components: {student}                          //局部注册组件，es6式(标签名：组件名)
```



子级给父级传递数据

```js
//App.vue
<Student v-on:name="fun" />
methods: {
    fun(v){
        console.log(v)
    }
}

//Student.vue
<button @click="funs"></button>
methods: {
    funs(){
        this.$emit('name','helloWorld!')         //触发App中name事件并传递数据
    }
}
-------------------------------------------
//App.vue
<Student :name="fun" />
methods: {
    fun(v){
        console.log(v)
    }
}

//Student.vue
<button @click="funs"></button>
props: ["name"],
methods: {
    funs(){
        this.name('helloWorld!')
    }
}
```

父级向子级传递数据

```js
//App.vue
<Student :name="name" age="19" />         //不加:是传递当前值
data(){
    return{
        name: 'wangluqi',
    }
}

//Student.vue
props: ['name','age']                     //导入数据
-----------------------
props: {                                  //导入数据并限制类型
    name: String,
    age: Number
},
data(){
    return {
        value: this.name
    }
}
```



消息订阅

> 提前安装pubsub-js包

```js
//Student.vue
import pubsub from "pubsub-js";
methods: {
    fun(_,v){                 //消息名(用_占位) 内容
        console.log("School向Student传递成功"+v)
    }
},
mounted(){
    this.pubId = pubsub.subscribe("name",this.fun)
},
beforeDestroy(){                        
    pubsub.unsubscribe(this.pubId);
}

//School.vue
<button @click="funs">按钮</button>
import pubsub from "pubsub-js";
data(){
    name:'wang'
}
methods:{
    funs(){
        pubsub.publish("name",this.name)
    }
}
```

