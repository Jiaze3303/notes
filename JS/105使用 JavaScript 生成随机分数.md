# 使用 JavaScript 生成随机分数

随机数非常适合用来创建随机行为。

在 JavaScript 中，可以用 `Math.random()` 生成一个在`0`（包括 0）到 `1`（不包括 1）之间的随机小数。 因此 `Math.random()` 可能返回 `0`，但绝不会返回 `1`。

**提示：**[使用赋值运算符存储值](https://chinese.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-javascript/storing-values-with-the-assignment-operator)这一节讲过，所有函数调用将在 `return` 执行之前结束，因此我们可以 `return`（返回）`Math.random()` 函数的值。
