# 给 JavaScript 对象添加新属性

你也可以像更改属性一样给 JavaScript 对象添加属性。

这里展示了如何给 `ourDog` 添加一个属性 `bark`：

```js
ourDog.bark = "bow-wow";
```

或者

```js
ourDog["bark"] = "bow-wow";
```

现在，当我们执行 `ourDog.bark` 时，就能得到他的叫声，`bow-wow`。

例如：

```js
const ourDog = {
  "name": "Camper",
  "legs": 4,
  "tails": 1,
  "friends": ["everything!"]
};

ourDog.bark = "bow-wow";
```
