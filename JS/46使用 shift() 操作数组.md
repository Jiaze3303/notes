`pop()` 函数用来移出数组中最后一个元素。 如果想要移出第一个元素要怎么办呢？

这时候我们就需要 `.shift()` 了。 它的工作原理就像 `.pop()`，但它移除的是第一个元素，而不是最后一个。

示例：

```js
const ourArray = ["Stimpson", "J", ["cat"]];
const removedFromOurArray = ourArray.shift();
```

`removedFromOurArray` 值为 `Stimpson`，`ourArray` 值为 `["J", ["cat"]]`
