# 创建 JavaScript 对象

你之前可能听过 `object` 这个词。

对象和 `arrays` 类似，区别在于数组使用索引来访问和修改数据，而对象中的数据是通过 `properties` 访问的。

对象非常适合用来存储结构化数据，可以表示真实世界中的物体，比如一只猫。

这里是一个猫对象的样本：

```js
const cat = {
  "name": "Whiskers",
  "legs": 4,
  "tails": 1,
  "enemies": ["Water", "Dogs"]
};
```

在此示例中，所有属性都存储为字符串，例如 `name`、`legs` 和 `tails`。 然而，你也可以使用数字作为属性。 你甚至可以省略单字字符串属性中的引号，如下所示：

```js
const anotherObject = {
  make: "Ford",
  5: "five",
  "model": "focus"
};
```

然而，如果你的对象有非字符串属性的话，JavaScript 会自动将它们转为字符串。
