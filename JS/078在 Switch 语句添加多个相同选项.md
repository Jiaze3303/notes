如果你忘了给 `switch` 的每一条 `case` 添加 `break`，那么后续的 `case` 会一直执行，直到遇见 `break` 为止。 如果你想为 `switch` 中的多个不同的输入设置相同的结果，可以这样写：

```js
let result = "";
switch (val) {
  case 1:
  case 2:
  case 3:
    result = "1, 2, or 3";
    break;
  case 4:
    result = "4 alone";
}
```

这样，1、2、3 都会有相同的结果。


