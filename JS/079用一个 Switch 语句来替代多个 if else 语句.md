如果你有多个选项需要选择，`switch` 语句写起来会比多个串联的 `if`/`else if` 语句容易些。 譬如:

```js
if (val === 1) {
  answer = "a";
} else if (val === 2) {
  answer = "b";
} else {
  answer = "c";
}
```

可以被下面替代：

```js
switch (val) {
  case 1:
    answer = "a";
    break;
  case 2:
    answer = "b";
    break;
  default:
    answer = "c";
}
```
