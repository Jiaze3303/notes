在 `switch` 语句中，你可能无法用 `case` 枚举出所有可能的值。 相反，你可以添加 `default` 语句，它会在找不到相匹配的 `case` 语句之后执行。 你可以把它看作是 `if/else` 链中最后的那个 `else` 语句。

`default` 语句应该被放到最后。

```js
switch (num) {
  case value1:
    statement1;
    break;
  case value2:
    statement2;
    break;
...
  default:
    defaultStatement;
    break;
}
```
