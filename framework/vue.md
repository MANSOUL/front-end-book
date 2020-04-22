## Vue  

### 组件生命周期

```js
new Vue({
    beforeCreate() {
        // 在事件&生命周期初始化后调用
    },
    created() {
        // 在数据劫持后调用
    },
    beforeMount() {
        // 模版编译完成后调用
    },
    mounted() {
        // 挂载后调用
    },
    beforeUpdate() {
        // 更新前调用
    },
    updated() {
        // 重新渲染后调用
    },
    beforeDestroy() {
        // 销毁组件观察者、子组件、事件监听前调用
    },
    destroyed() {
        // 销毁组件后调用
    },
    activited() {
        // keep-alive 专属，组件被激活时调用
    },
    deactivated() {
        // keep-alive 专属，组件被销毁时调用
    }
})
```

![Vue 生命周期](../images/6.jpg)

### 指令

- `v-if`, `v-else-if`, `v-else`
- `v-show`
- `v-for`
- `v-on`
- `v-bind`
- `v-model`

### 