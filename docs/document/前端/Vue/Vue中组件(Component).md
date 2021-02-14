# Vue 中组件(Component)

## 1. 组件作用

用来减少`Vue`实例对象中代码量，日后在使用`Vue`开发过程中，可以根据不同业务功能将页面中划分不同的多个组件，然后由多个组件去完成整个页面的布局，便于日后使用`Vue`进行开发时页面管理，方便开发人员维护。

## 2. 组件使用

### 2.1. 全局组件

**说明：全局组件注册给 Vue 实例,日后可以在任意 Vue 实例的范围内使用该组件**

```js
      //1.开发全局组件
	  Vue.component("userLogin", {
        template: "<div><h1>用户登入</h1></div>",
      });

	  //使用全局组件  在Vue实例范围内
      <!--驼峰命名规则注意-->
      <user-login></user-login>
    </div>
```

![image-20210214124628573](<media/Vue中组件(Component).assets/image-20210214124628573.png>)

> [!tip]
>
> 1. `Vue.component`用来开发全局组件 参数 1: 组件的名称 参数 2: 组件配置{} template:''用来书写组件的 html 代码 template 中必须有且只有一个 root 元素
> 2. 使用时需要在`Vue`的作用范围内根据组件名使用全局组件
> 3. **如果在注册组件过程中使用 驼峰命名组件的方式 在使用组件时 必须将驼峰的所有单词小写加入-线进行使用**

### 2.2. 局部组件

**说明：过将组件注册给对应 Vue 实例中一个 components 属性来完成组件注册,这种方式不会对 Vue 实例造成累加**

- 第一种开发方式

  ```js
  //局部组件登录模板声明
  let login = {
    //具体局部组件名称
    template: "<div><h2>用户登录</h2></div>",
  };

  const app = new Vue({
    el: "#app",
    data: {},
    methods: {},
    components: {
      //用来注册局部组件
      login: login, //注册局部组件
    },
  });

  //局部组件使用 在Vue实例范围内
  <login></login>;
  ```

- 第二种开发方式

  ```js
  //1.声明局部组件模板  template 标签 注意:在Vue实例作用范围外声明
  <template id="loginTemplate">
    <h1>用户登录</h1>
  </template>;

  //2.定义变量用来保存模板配置对象
  let login = {
    //具体局部组件名称
    template: "#loginTemplate", //使用自定义template标签选择器即可
  };

  //3.注册组件
  const app = new Vue({
    el: "#app",
    data: {},
    methods: {},
    components: {
      //用来注册局部组件
      login: login, //注册局部组件
    },
  });

  //4.局部组件使用 在Vue实例范围内
  <login></login>;
  ```

## 3. Prop 的使用

**作用：`props`用来给组件传递相应静态数据或者是动态数据的**

### 3.1. 通过在组件上声明静态数据传递给组件内部

```js
//1.声明组件模板配置对象
let login = {
  template: "<div><h1>欢迎:{{ userName }} 年龄:{{ age }}</h1></div>",
  props: ["userName", "age"], //props作用 用来接收使用组件时通过组件标签传递的数据
};

//2.注册组件
const app = new Vue({
  el: "#app",
  data: {},
  methods: {},
  components: {
    login, //组件注册
  },
});

//3.通过组件完成数据传递
<login user-name="千仔" age="23"></login>;
```

![image-20210214124912822](<media/Vue中组件(Component).assets/image-20210214124912822.png>)

> [!tip]
>
> 1. 使用组件时可以在组件上定义多个属性以及对应数据
> 2. 在组件内部可以使用`props`数组生命多个定义在组件上的属性名 日后可以在组件中通过`{{ 属性名 }} `方式**获取组件中属性值**

### 3.2. 通过在组件上声明动态数据传递给组件内部

```js
//1.声明组件模板对象
    const login = {
        template:'<div><h2>欢迎: {{ name }} 年龄:{{ age }}</h2></div>',
        props:['name','age']
    }

//2.注册局部组件
    const app = new Vue({
        el: "#app",
        data: {
            username:"千仔",
            age:18
        },
        methods: {},
        components:{
            login //注册组件
        }
    });

//3.使用组件
	 <login :name="username" :age="age"></login>  //使用v-bind形式将数据绑定Vue实例中data属性,日后data属性发生变化,组件内部数据跟着变化
```

![image-20210214124338626](<media/Vue中组件(Component).assets/image-20210214124338626.png>)

### 3.3. prop 的单向数据流

!>[单向数据流——Vue 官网](https://cn.vuejs.org/v2/guide/components-props.html#%E5%8D%95%E5%90%91%E6%95%B0%E6%8D%AE%E6%B5%81)

**单向数据流**：所有的 `prop `都使得其父子 `prop `之间形成了一个**单向下行绑定**：父级 `prop `的更新会向下流动到子组件中，但是反过来则不行。

> - 这样会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。
> - 每次父级组件发生变更时，子组件中所有的 `prop `都将会刷新为最新的值。
> - 这意味着你不应该在一个子组件内部改变 `prop`。如果你这样做了，`Vue `会在浏览器的控制台中发出警告。

## 组件中定义数据和事件

### 组件中定义属于组件的数据

```js
//组件声明的配置对象
    const login = {
        template:'<div><h1>{{ msg }} Vue学习中</h1><ul><li v-for="item,index in lists">{{ index }}{{ item }}</li></ul></div>',
        data(){   //使用data函数方式定义组件的数据   在templatehtml代码中通过插值表达式直接获取
            return {
                msg:"hello",
                lists:['java','spring','springboot']
            }//组件自己内部数据
        }
    }
```



### 组件中事件定义

```js
  const login = {
        template :'<div><input type="button" value="点我" @click="change"></div>'
        data(){
            return {
                name:'小千'
            };
        },
        methods:{
            change(){
                alert(this.name)
                alert('触发事件');
            }
        }
    };
```

> [!tip]
>
> 1. 组件中定义事件和直接在`Vue`中定义事件基本一致 直接在组件内部对应的`html`代码上加入`@事件名=函数名`方式即可
> 2. 在组件内部使用`methods`属性用来定义对应的事件函数即可,事件函数中`this`指向的是当前组件的实例









---

