上一个挑战是 JSX 的一个简单示例，但 JSX 也可以表示更复杂的 HTML。

关于嵌套的 JSX，需要知道的一件重要的事情，那就是它必须返回单个元素。

这个父元素将包裹所有其他级别的嵌套元素。

例如，几个作为兄弟元素编写的 JSX 元素而没有父元素包裹将不会被转换。

这里是一个示例：

**有效的 JSX：**

```jsx
<div>
  <p>Paragraph One</p>
  <p>Paragraph Two</p>
  <p>Paragraph Three</p>
</div>
```

**无效的 JSX：**

```jsx
<p>Paragraph One</p>
<p>Paragraph Two</p>
<p>Paragraph Three</p>
```
