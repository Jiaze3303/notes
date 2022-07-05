改变数组中数据的另一种方法是用 `.pop()` 函数。

`.pop()` 函数用来弹出一个数组末尾的值。 我们可以把这个弹出的值赋给一个变量存储起来。 换句话说就是 `.pop()` 函数移除数组末尾的元素并返回这个元素。

数组中任何类型的元素（数值，字符串，甚至是数组）都可以被弹出来 。

```js
const threeArr = [1, 4, 6];
const oneDown = threeArr.pop();
console.log(oneDown);
console.log(threeArr);
```

第一个 `console.log` 将显示值 `6`，第二个将显示值 `[1, 4]`。


