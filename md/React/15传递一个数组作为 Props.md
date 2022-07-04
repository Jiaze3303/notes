上一个挑战演示了如何将来自父组件的信息作为 `props` 传递给子组件。 这个挑战着眼于如何将数组作为 `props` 传递。 要将数组传递给 JSX 元素，必须将其视为 JavaScript 并用花括号括起来。

```jsx
<ParentComponent>
  <ChildComponent colors={["green", "blue", "red"]} />
</ParentComponent>
```

这样，子组件就可以访问数组属性 `colors`。 访问属性时可以使用 `join()` 等数组方法。 `const ChildComponent = (props) => <p>{props.colors.join(', ')}</p>` 这将把所有 `colors` 数组项连接成一个逗号分隔的字符串并生成： `<p>green, blue, red</p>` 稍后，我们将了解在 React 中渲染数组数据的其他常用方法。
