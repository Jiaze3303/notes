# while 循环

你可以使用循环多次执行相同的代码。

我们将学习的第一种类型的循环称为 `while` 循环，当 while 指定的条件为真，循环才会执行，反之不执行。

```js
const ourArray = [];
let i = 0;

while (i < 5) {
  ourArray.push(i);
  i++;
}
```

在上面的代码里，`while` 循环执行 5 次把 0 到 4 的数字添加到 `ourArray` 数组里。

让我们通过 while 循环将值添加到数组中。
