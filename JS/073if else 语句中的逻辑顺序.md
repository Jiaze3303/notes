`if`、`else if` 语句中的代码顺序是很重要的。

在条件判断语句中，代码的执行顺序是从上到下，所以你需要考虑清楚先执行哪一句，后执行哪一句。

这有两个例子。

第一个例子：

```js
function foo(x) {
  if (x < 1) {
    return "Less than one";
  } else if (x < 2) {
    return "Less than two";
  } else {
    return "Greater than or equal to two";
  }
}
```

第二个例子更改了代码的执行顺序：

```js
function bar(x) {
  if (x < 2) {
    return "Less than two";
  } else if (x < 1) {
    return "Less than one";
  } else {
    return "Greater than or equal to two";
  }
}
```

这两个函数看起来几乎一模一样，我们传一个值进去看看它们有什么区别。

```js
foo(0)
bar(0)
```

`foo(0)` 将返回字符串 `Less than one`，`bar(0)` 将返回字符串 `Less than two`。
