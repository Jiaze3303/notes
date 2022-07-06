# 函数执行到 return 语句就结束

当代码执行到 `return` 语句时，函数返回一个结果就结束运行了，return 后面的语句不会执行。

**示例**

```js
function myFun() {
  console.log("Hello");
  return "World";
  console.log("byebye")
}
myFun();
```

以上将在控制台中显示字符串 `Hello` 并返回字符串 `World`。 字符串 `byebye` 将永远不会在控制台中显示，因为函数在 `return` 语句处就退出了。
