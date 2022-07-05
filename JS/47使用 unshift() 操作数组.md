不仅可以 `shift`（移出）数组中的第一个元素，也可以 `unshift`（移入）一个元素到数组的头部。

`.unshift()` 函数用起来就像 `.push()` 函数一样，但不是在数组的末尾添加元素，`unshift()` 在数组的头部添加元素。

示例：

```js
const ourArray = ["Stimpson", "J", "cat"];
ourArray.shift();
ourArray.unshift("Happy");
```

在 `shift`、`ourArray` 后值为 `["J", "cat"]`。 在 `unshift`、`ourArray` 后值为 `["Happy", "J", "cat"]`。
