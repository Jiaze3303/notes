函数一般用 `return` 语句来返回值，但这不是必须的。 在函数没有 `return` 语句的情况下，当你调用它时，该函数会执行内部代码，返回的值是 `undefined`。

**示例**

```js
let sum = 0;

function addSum(num) {
  sum = sum + num;
}

addSum(3);
```

`addSum` 是一个没有 `return` 语句的函数。 该函数将更改全局变量 `sum`，函数的返回值为 `undefined`。
