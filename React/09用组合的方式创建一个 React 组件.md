现在来看看如何组合多个 React 组件。 想象一下，现在正在构建一个应用程序，并创建了三个组件：`Navbar`、`Dashboard` 和 `Footer`。

要将这些组件组合在一起，可以创建一个 `App` *父组件*，将这三个组件分别渲染成为*子组件*。 要在 React 组件中渲染一个子组件，需要在 JSX 中包含作为自定义 HTML 标签编写的组件名称。 例如，在 `render` 方法中，可以这样编写：

```jsx
return (
 <App>
  <Navbar />
  <Dashboard />
  <Footer />
 </App>
)
```

当 React 遇到一个自定义 HTML 标签引用另一个组件的时（如本例所示，组件名称包含在 `< />` 中），它在自定义标签的位置渲染该组件的标签。 这可以说明 `App` 组件和 `Navbar`、`Dashboard` 以及 `Footer` 之间的父子关系。
