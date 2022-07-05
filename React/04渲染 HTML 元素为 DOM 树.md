到目前为止，已经了解到 JSX 是一种在 JavaScript 中编写可读 HTML 的便捷工具。 在 React 中，可以使用它的的渲染 API（ReactDOM）将此 JSX 直接渲染到 HTML DOM。

ReactDOM 提供了一个简单的方法来将 React 元素呈现给 DOM，如下所示：`ReactDOM.render(componentToRender, targetNode)`，其中第一个参数是要渲染的 React 元素或组件，第二个参数是组件将要渲染到的 DOM 节点。

如你所料，必须在 JSX 元素声明之后调用 `ReactDOM.render()`，就像在使用变量之前必须声明它一样。


