ES6 引入了展开操作符，可以展开数组以及需要多个参数或元素的表达式。

下面的 ES5 代码使用了 `apply()` 来计算数组的最大值：

```js
var arr = [6, 89, 3, 45];
var maximus = Math.max.apply(null, arr);
```

`maximus` 的值为 `89`。

我们必须使用 `Math.max.apply(null, arr)`，因为 `Math.max(arr)` 返回 `NaN`。 `Math.max()` 函数中需要传入的是一系列由逗号分隔的参数，而不是一个数组。 展开操作符可以提升代码的可读性，使代码易于维护。

```js
const arr = [6, 89, 3, 45];
const maximus = Math.max(...arr);
```

`maximus` 的值应该是 `89`。

`...arr` 返回一个解压的数组。 也就是说，它*展开*数组。 然而，展开操作符只能够在函数的参数中或者数组中使用。 下面的代码将会报错：

```js
const spreaded = ...arr;
```
