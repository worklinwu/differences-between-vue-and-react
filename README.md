# 对比 Vue 和 React 的差异

**[目录]**
- [写在前面](#写在前面)
- [创建一个组件](#创建一个组件)
- [更新状态:](#更新状态)
- [生命周期](#生命周期)
- [空白组件](#空白组件)
- [指定挂载](#指定挂载)
- [Hooks](#hooks)
- [事件处理](#事件处理)
- [自定义事件](#自定义事件)
- [函数式组件](#函数式组件)
- [Vuex 与 Redux](#vuex-与-redux)
- [Computed(TODO)](#computedtodo)
- [Watch(TODO)](#watchtodo)
- [Router(TODO)](#routertodo)

### 写在前面

我想很多人和我一样, 先学会了 Vue 或者 React, 然后再去学另外一个, 但是突然发现两者的实现在思维上还是有很大差异的, 然后就开始嫌弃另外一个语言, 觉得哪哪不如我先学的这个.  
我在学习了 Vue 之后再去学习 React 的. 发现如果能找到两者的相似和差异之处, 理解起来会更快, 所以做了下面一些总结, 希望对你有帮助.  

时间有限, 文章不会一步到位, 会持续更新. 欢迎大家关注, 或者参与进来. 有错误的地方也欢迎指正.  

[Github地址](https://github.com/worklinwu/differences-between-vue-and-react)

### 创建一个组件

  ```jsx
  // vue
  // Vue.component('my-component-name', {})
  new Vue({
    name: 'DemoComponent',
    props: { // 定义属性
       bar: {
        type: String, // 定义类型
        required: true, // 必填
        default: '1', // 默认值
        validator: function (value) { // 自定义验证
          return ['a', 'b'].includes(value)
        }
      }
    },
    data() {
      return { foo: '1' } // 初始化参数
    },
    template: '<div>{{ foo }} {{ bar }}</div>', // 渲染方式
    // vue 的 jsx 使用方式
    // render() { return <div>{a}</div>}
  })

  // react
  export class DemoComponent extends React.Component {
    // getDefaultProps()
    DemoComponent.defaultProps = {
      bar: '1' // 默认值
    };
    DemoComponent.propTypes = {
      bar: React.PropTypes.string.isRequired.oneOf(['a', 'b']) // 定义类型.必填.自定义验证
    }
    constructor(props){
      super(props);
      // getInitialState()
      this.state = { foo: '1' } // 初始化参数
    }
    render() {
      return (<div>{foo} {bar}</div>); // 渲染方式
    }
  }
  ```

### 更新状态:

  ```js
  // vue
  this.foo = 'demo'
  this.$data.foo = 'demo'
  this.$data.arrayFoo.push('bar')
  
  // react
  this.setState({foo: "demo"})
  this.setState((state, props) => { foo: state.foo + props.bar })
  this.setState({arrayFoo: [...this.state.arrayFoo, 'bar']})
  ```

### 生命周期
  
```jsx
beforeCreate => [没有]
created => [没有]
beforeMounted => componentWillMount
mounted => componentDidMount
beforeUpdate => componentWillUpdate
updated => componentDidUpdate
[没有] => shouldComponentUpdate
activated => [没有]
deactivated => [没有]
beforeDestroy => componentWillUnmount
destroyed => [没有]
errorCaptured => * componentDidCatch
* watch函数 => componentWillReceiveProps(nextProps)
```

### 空白组件

`<template>` 相当于 `<React.Fragment>` 或者 `<>`

### 指定挂载

`vm.$mount('#domid')` 类似 `ReactDOM.createPortal`, 可以在指定位置挂载组件内容

### Hooks

vue3.x 的 `composition-api` 类似 `Hooks`
  
```js
// vue
{ reactive, computed, toRefs } from '@vue/composition-api'

// react
import { useState，useEffect，useContext，useReducer } from "react";
```

### 事件处理

```jsx
// vue
<div @click="handleClick('arg1')"></div>
// react
<div onClick={this.handleClick.bind(this, 'arg1')}></div>
```

### 自定义事件

```jsx
// vue
<div @customEventName='handleCustomEvent'></div>
// this.$emit('handleCustomEvent', ...)

// react
// TODO
```

### 函数式组件
  
```jsx
// vue
Vue.component('my-component', {
  functional: true,
}
<template functional></template>

// react
MyComponent = () => {
  return(<div>test</div>)
}
```

### Vuex 与 Redux

```js
// Action
action => Action Creators
// vue
example(context, payload) {
  context.commit('EXAMPLE_METHOD', payload)
}
// react
function example(data) {
  return { type:"EXAMPLE_METHOD", payload: data};
}

// Mutations
mutations => Reducers
// vue
EXAMPLE_METHOD (state, payload) {
  state.foo = payload
}
// react
function demoReducer(state = [], action){
  switch(action.type) {
    case "EXAMPLE_METHOD":
      return Object.assign({}, state, {foo: action.payload});
    default: return state;
  }
}

// map
mapState/mapAction =>
  const mapStateToProps = (state, ownProps) => ({xx: state.xx})
  const mapDispatchToProps = (dispatch) => ({actions: bindActionCreators(customActions, dispatch)})
  // this.state.xx
  // this.props.actions.yy
  connect(mapStateToProps, mapDispatchToProps)(MyComponent)
```

### Computed(TODO)

### Watch(TODO)

### Router(TODO)

...
