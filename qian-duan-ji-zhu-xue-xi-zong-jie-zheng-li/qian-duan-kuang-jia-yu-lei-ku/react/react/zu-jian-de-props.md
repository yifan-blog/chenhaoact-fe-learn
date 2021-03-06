###组件的props

从父级传来的数据在子组件里作为 '属性' （类似于html标签里属性）可供使用。 这些 '属性' 可以通过`this.props`访问 

#### （1）Prop 验证（通过propTypes和defaultProps，目前已经从react分离出来，需要单独安装prop-types）

随着应用不断变大，保证组件被正确使用变得非常有用。**React.PropTypes 提供很多验证器 \(validator\) 来验证传入数据的有效性。**当向 props 传入无效数据时，JavaScript 控制台会抛出警告。注意为了性能考虑，只在开发环境验证 propTypes。下面用例子来说明不同验证器的区别：

在组件中写入：

```
import PropTypes from 'prop-types';

...
static propTypes = {
  name: PropTypes.string,
}

static defaultProps = {
  name: '',
}
...

```
propTypes定义props的类型等校验规则，defaultProps定义默认的props。

写（有props传入的）组件的时候最好把propTypes和defaultProps都定义上，这样就清楚了该组件到底能传入什么样的props,类型是什么，作用是什么，无论是对代码的稳定性还是可读性都有帮助。

具体参考：https://react.bootcss.com/react/docs/typechecking-with-proptypes.html

#### （2）Single Child

用 **React.PropTypes.element 可以指定仅有一个子级**能被传送给组件。

```
var MyComponent = React.createClass({
  propTypes: {
    children: React.PropTypes.element.isRequired
  },

  render: function() {
    return (
      <div>
        {this.props.children} // 这里必须是一个元素否则就会警告
      </div>
    );
  }

});
```

#### （3）默认 Prop 值

React 支持以声明式的方式来定义 props 的默认值。

```
var ComponentWithDefaultProps = React.createClass({
  getDefaultProps: function() {
    return {
      value: 'default value'
    };
  }
  /* ... */
});
```

当父级没有传入 props 时，**getDefaultProps\(\) **可以保证 this.props.value 有默认值，注意 getDefaultProps 的结果会被 缓存。得益于此，你可以直接使用 props，而不必写手动编写一些重复或无意义的代码。

#### （4）传递props

##### 1\)手动传递

大部分情况下你应该显式地向下传递 props。**这样可以确保只公开你认为是安全的内部 API 的子集**。  
下面就手动把FancyCheckbox的props传给了div：

```
function FancyCheckbox(props) {
  var fancyClass = props.checked ? 'FancyChecked' : 'FancyUnchecked';
  return (
    <div className={fancyClass} onClick={props.onClick}>
      {props.children}
    </div>
  );
}
ReactDOM.render(
  <FancyCheckbox checked={true} onClick={console.log.bind(console)}>
    Hello world!
  </FancyCheckbox>,
  document.getElementById('example')
);
```

##### 2）传递全部props

有一些常用的 React 组件只是对 HTML 做简单扩展。通常，你想**复制任何传进你的组件的HTML属性到底层的HTML元素上**。为了减少输入，你可以用 JSX spread（扩展运算符...） 语法来完成：

```
var CheckLink = React.createClass({
  render: function() {
    // 这样会把 CheckList 所有的 props 复制到 <a>
    return <a {...this.props}>{'√ '}{this.props.children}</a>;
  }
});

ReactDOM.render(
  <CheckLink href="http://www.react-cn.com/checked.html">
    Click here!
  </CheckLink>,
  document.getElementById('example')
);
```

