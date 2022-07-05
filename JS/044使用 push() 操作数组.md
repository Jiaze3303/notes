一个将数据添加到数组末尾的简单方法是 `push()` 函数。

`.push()` 接受一个或多个参数（parameters），并把它压入到数组的末尾。

示例：

```js
const arr1 = [1, 2, 3];
arr1.push(4);

const arr2 = ["Stimpson", "J", "cat"];
arr2.push(["happy", "joy"]);
```

`arr1` 现在值为 `[1, 2, 3, 4]`，`arr2` 值为 `["Stimpson", "J", "cat", ["happy", "joy"]]`。


