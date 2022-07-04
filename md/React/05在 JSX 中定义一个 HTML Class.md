现在已经习惯了编写 JSX，可能想知道它与 HTML 有什么不同。

到目前为止，HTML 和 JSX 似乎完全相同。

JSX 的一个关键区别是你不能再使用 `class` 这个单词来做为 HTML 的 class 名。 这是因为 `class` 是 JavaScript 中的关键字。 而 JSX 使用 `className` 来代替。

事实上，JSX 中所有 HTML 属性和事件引用的命名约定都变成了驼峰式。 例如，JSX 中的单击事件是 `onClick`，而不是 `onclick`。 同样，`onchange` 变成了`onChange`。 虽然这是一个微小的差异，但请你一定要记住。


