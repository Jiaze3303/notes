还记不记得在之前的挑战中使用 ReactDOM API 将 JSX 元素渲染到 DOM， 这与渲染 React 组件的过程十分相似。 过去的几个挑战主要针对组件和组合，因此渲染是在幕后完成的。 但是，如果不调用 ReactDOM API，编写的任何 React 代码都不会渲染到 DOM。

复习一下语法： `ReactDOM.render(componentToRender, targetNode)`。 第一个参数是要渲染的 React 组件。 第二个参数是要在其中渲染该组件的 DOM 节点。

传递到`ReactDOM.render()` 的React 组件与 JSX 元素略有不同。 对于 JSX 元素，传入的是要渲染的元素的名称。 但是，对于 React 组件，需要使用与渲染嵌套组件相同的语法，例如`ReactDOM.render(<ComponentToRender />, targetNode)`。 此语法用于 ES6 class 组件和函数组件都可以。
