# fe-coding-style-guide
Coding style guide of frontend team.

# 参考指南
> 指南仅供参考

## 注释
1. 模块或者组件的关键代码部分添加必要的注释说明
2. 通用组件的不常见属性(props)添加必要的注释说明

## 基于模块/组件开发
始终基于模块/组件的方式来构建 app，每一个子模块/子组件只做一件事情。复杂的模块或者组件应遵循单一职责原则进行细粒度拆分，保证子组件是独立的、可复用的和可测试的。

## 命名
**1. 图片等资源命名:** 符合短横线命名法(kebab-case)规则且有含义，类似或同类资源可加数字编号予以区分：

```
// bad
a.png/b_ss.png

// good
avatar.png/broker1.png/fxcm-icon.png
```

**2. 组件命名:** 符合短横线命名法(kebab-case)规则且有含义, 且单词长度不超过3个:

```
<script>
  export default {
      name: 'fm-dialog'
      // ...
  }
</script>
```

**3. css 类命名:** 项目自定义前缀且符合短横线命名法(kebab-case)规则, 组合单词长度不超过5个：

```
// bad
.at_tipbox/.titletxt

// good
.trade-at-tip-box/.trade-title-text
```
**4. 单文件组件:** 组件命名采用小写字母+中划线的组合且有含义, 且单词长度不超过3个; 如 `dialog.vue`/`cancel-button.vue`，且单文件组件的 `.vue` 文件和 `less` 文件分开, 推荐以下列方式引入对应的样式文件:

```
// dialog.vue
<script>
  import './index.less';
</script>
```

**5. 引用名:** 组件引用采用帕斯卡(PascalCase)命名法:

```
// bad
import reservationCard from './reservationCard';

// good
import ReservationCard from './reservation-card';
```

**6. 其它:** 过滤器和函数采用驼峰式(CamelCase)命名法，且函数类的首字母大写：

## props 属性
**1. props 原子化**: 尽量只使用 [JavaScript 原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)（字符串、数字、布尔值）和函数, 避免复杂的对象, 保持组件 API 简洁直观.

```
// bad
<fm-input :config="complexConfigObject"></fm-input>

// good
<fm-input
  :values="20"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</fm-input>
```

**2. props 验证**: props 的推荐定义和验证请参考 [props 验证](https://cn.vuejs.org/v2/guide/components.html#Prop-验证).

```
// bad
export default {
    props: ['type']
}

// good
export default {
    props: {
        type: {
            type: String,
            default: 'normal',
            required: true,
            validator: (val) => ['normal', 'small', 'large'].includes(val)
        }
    }
}
```

## 数据通信
**1. 遵循「单向数据流原则」**
**2. 父子组件通信遵循 「props向下传递,事件向上传递」, 避免使用 $this.parent**
**3. 遵循 MVVM 框架的「数据驱动原则」, 减少或者避免DOM操作, 避免使用$set/$refs等 API**
**4. 善用 [「Object.assign」](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 和解构来赋予对象新值**

```
// 更新对象的值
// very bad
const original = { a: 1, b: 2 };
original.a = 2;

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { a: 2})  // 这种方式会改变 original

// good
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { a: 2}) // 这种方式不会改变 original

// 对象添加新属性
// very bad
const original = { a: 1, b: 2 };
original.c = 3;

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3})  // 这种方式会改变 original

// good
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3})  // 不会改变 original

// very good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3} 
```

## 组件结构化
单文件组件推荐按照结构化的方式进行编写：

#### Vue单文件组件:

```
<template>
  // ...
</template>

<script>
  //0. 组件样式
  import './index.less';

  //1. 三方模块
  import vue from 'vue';
  import Cookies from 'js-cookies';
  import xxx from 'yyy';

  //2. 业务功能模块
  import xxx from '@src/lib/utils'
  import yyy from '@src/lib/filter'

  //3. 组件模块
  import ShownTime from '@src/components/show-time/index';
  import XxxDialog from '@src/components/xxx-dialog/index';

  export default {
     // 属性顺序
     props
     data
     hooks(beforeRouteEnter.etc.)
     computed
     watch
     methods
     // 生命周期
     beforeCreate
     created
     beforeMount
     mounted
     beforeUpdate
     updated
     beforeDestroy
     destroyed
     components
  }
</script>  
```

#### React单文件组件:

```
  //0. 组件样式
  import './index.less';

  //1. 三方模块
  import vue from 'vue';
  import Cookies from 'js-cookies';
  import xxx from 'yyy';

  //2. 业务功能模块
  import xxx from '@src/lib/utils'
  import yyy from '@src/lib/filter'

  //3. 组件模块
  import ShownTime from '@src/components/show-time/index';
  import XxxDialog from '@src/components/xxx-dialog/index';

  export default class MyComponent extends React.Component {
      constructor
      static props
      static methods
      componentWillMount
      componentDidMount
      componentWillReceiveProps
      shouldComponentUpdate
      componentWillUpdate
      componentDidUpdate
      componentWillUnmount
      clickHandlers or eventHandlers.etc
      getter methods for render: getSelectReason() or getFooterContent()
      Optional render methods: enderNavigation() or renderProfilePicture()   
      render 
  }
```
