React 提供了有用的类型检查特性，以验证组件是否接收了正确类型的 props。 例如，应用程序调用 API 来检索数据是否是数组，然后将数据作为 prop 传递给组件。 可以在组件上设置 `propTypes`，以要求数据的类型为 `array`。 当数据是任何其它类型时，都会抛出警告。

当提前知道 prop 的类型时，最佳实践是设置其 `propTypes`。 可以为组件定义 `propTypes` 属性，方法与定义 `defaultProps` 相同。 这样做将检查给定键的 prop 是否是给定类型。 这里有一个示例，表示名为 `handleClick` 的 prop 应为 `function` 类型：

```js
MyComponent.propTypes = { handleClick: PropTypes.func.isRequired }
```

在上面的示例中，`PropTypes.func` 部分检查 `handleClick` 是否为函数。 添加 `isRequired`，告诉 React `handleClick` 是该组件的必需属性。 如果没有那个属性，将出现警告。 还要注意 `func` 代表 `function` 。 在 7 种 JavaScript 原语类型中， `function` 和 `boolean` （写为 `bool` ）是唯一使用异常拼写的两种类型。 除了原始类型，还有其他类型可用。 例如，你可以检查 prop 是否为 React 元素。 请参阅[文档](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes)以获取所有选项。

**注意：**在 React v15.5.0 中, `PropTypes` 可以从 React 中单独引入，例如：`import PropTypes from 'prop-types';`。
