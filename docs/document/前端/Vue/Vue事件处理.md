# Vue事件处理

!>[官方文档地址](https://cn.vuejs.org/v2/guide/events.html)

## 事件修饰符

**常用事件修饰符**

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

### stop

!>用来阻止事件冒泡

如下，我们在触发`btnClick`事件的时候，会对外层的父元素进行事件冒泡，这时候，就需要`.stop`来阻止

```html
    <style>
      .aa {
        background: red;
        width: 300px;
        height: 300px;
      }
    </style>
...
...
    <div id="app" class="aa" @click="divClick">
      <input type="button" value="按钮" @click.stop="btnClick" />
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: "#app",
        data: {},
        methods: {
          btnClick() {
            alert("按钮被单击了");
          },
          divClick() {
            alert("div被点击了");
          },
        },
      });
    </script>
```

### prevent

!>用来阻止标签的默认行为

```html
    <div id="app">
      <!--
       .prevent : 用来阻止事件的默认行为
       -->
      <a href="https://www.baidu.com/" @click.prevent="aClick">百度一下</a>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: "#app",
        data: {},
        methods: {
          aClick() {
            alert("跳转百度");
          },
        },
      });
    </script>
```

### self

!>只触发自己标签上的事件，不监听冒泡事件

```html
    <div id="app">
      <!--只触发标签自身的事件-->
      <div class="aa" @click.self="divClick">
        <!--用来阻止事件冒泡-->
        <input type="button" value="按钮" @click.stop="btnClick" />
        <input type="button" value="按钮1" @click="btnClick1" />
      </div>
    </div>

    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: "#app",
        data: {},
        methods: {
          divClick() {
            alert("div被点击了");
          },
          btnClick() {
            alert("btnClick被点击了");
          },
          btnClick1() {
            alert("btnClick1被点击了");
          },
        },
      });
    </script>
```

### once

!>让指定事件只触发一次

```html
    <div id="app">
      <a href="https://www.baidu.com/" @click.prevent.once="aClick">百度一下</a>
    </div>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: "#app",
        data: {},
        methods: {
          aClick() {
            alert("跳转百度");
          },
        },
      });
    </script>
```

## 按键修饰符

!>**作用**：用来与键盘中按键事件绑定在一起，用来修饰特定的按键事件的修饰符

**常用的按键码的别名：**

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

### enter

!>用来在触发回车按键之后触发的事件

```html
 <input type="text" v-model="msg" @keyup.enter="keyups">
```

### tab

!>用来捕获到`tab`键执行到当前标签是才会触发

```html
<input type="text" @keyup.tab="keytabs">
```

