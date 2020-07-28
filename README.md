# 对比 Vue 和 React 的差异

## 写在前面

我想很多人和我一样, 先学会了 Vue 或者 React, 然后再去学另外一个, 但是突然发现两者的实现在思维上还是有很大差异的, 然后就开始嫌弃另外一个语言, 觉得哪哪不如我先学的这个.  
我在学习了 Vue 之后再去学习 React 的. 发现如果能找到两者的相似和差异之处, 理解起来会更快, 所以做了下面一些总结, 希望对你有帮助.  

时间有限, 文章不会一步到位, 会持续更新. 欢迎大家关注, 或者参与进来.  

[Github地址]()

- 创建一个组件

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

- 更新状态:

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

- 生命周期
  
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

- `<template>` 相当于 `<React.Fragment>` 或者 `<>`
- `vm.$mount('#domid')` 类似 `ReactDOM.createPortal`, 可以在指定位置挂载组件内容
- vue3.x 的 `composition-api` 类似 `Hooks`
  
  ```js
  // vue
  { reactive, computed, toRefs } from '@vue/composition-api'

  // react
  import { useState，useEffect，useContext，useReducer } from "react";
  ```

- 事件处理

  ```jsx
  // vue
  <div @click="handleClick('arg1')"></div>
  // react
  <div onClick={this.handleClick.bind(this, 'arg1')}></div>
  ```

- 自定义事件

  ```jsx
  // vue
  <div @customEventName='handleCustomEvent'></div>
  // this.$emit('handleCustomEvent', ...)

  // react
  // TODO
  ```

- 函数式组件.
  
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
