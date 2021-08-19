# react核心概念
## 事件处理
### 使用
1. 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。
```js
<button onClick={activateLasers}>  Activate Lasers> </button>
```
2. 在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。你必须显式的使用 `preventDefault`
```js
function ActionLink() {
   function handleClick(e) {   
     e.preventDefault();   
   }
  return (
    <a href="#" onClick={handleClick}>  Click me  </a>
  );
}
3. 用法上需要constructor里进行this绑定
```js
 constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
    this.handleClick = this.handleClick.bind(this); 
 }
 handleClick(){}
```
或
```js
  // 此语法确保 `handleClick` 内的 `this` 已被绑定。 
  handleClick = () => {    
    console.log('this is:', this);  
  }
```
原理： [JavaScript 函数工作原理](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)

### 向事件处理程序传递参数
```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
上述两种方式是等价的，分别通过[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)和 [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) 来实现。
## 列表 & Key
[深入解析为什么 key 是必须的](https://react.docschina.org/docs/reconciliation.html#recursing-on-children)   \
[深度解析使用索引作为 key 的负面影响](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)
### 用 key 提取组件
```js
function ListItem(props) {
  // 正确！这里不需要指定 key： 
  return <li>{props.value}</li>;}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 正确！key 应该在数组的上下文中被指定   
    <ListItem key={number.toString()}   value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
### 在 JSX 中嵌入 map()
```js
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>       
        <ListItem key={number.toString()}   value={number} />      )}
    </ul>
  );
}
```
这么做有时可以使你的代码更清晰，但有时这种风格也会被滥用。就像在 JavaScript 中一样，何时需要为了可读性提取出一个变量，这完全取决于你。但请记住，如果一个 `map()` 嵌套了太多层级，那可能就是你[提取组件](https://react.docschina.org/docs/components-and-props.html#extracting-components)的一个好时机。

## 表单
```js
this.setState({
  [name]: value
}); 
```
有时使用受控组件会很麻烦，因为你需要为数据变化的每种方式都编写事件处理函数，并通过一个 React 组件传递所有的输入 state。当你将之前的代码库转换为 React 或将 React 应用程序与非 React 库集成时，这可能会令人厌烦。在这些情况下，你可能希望使用[非受控组件](https://react.docschina.org/docs/uncontrolled-components.html), 这是实现输入表单的另一种方式。






