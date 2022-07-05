# 08 useReducer 基础用法

## useReducer 概念解释

我们第四个要学习的 Hook(钩子函数)是 useReducer，他的作用是“勾住”某些自定义数据对应的 dispatch 所引发的数据更改事件。useReducer 可以替代 useState，实现更为复杂逻辑的数据修改。

在 React 16.8 版本以前，通常需要使用第三方 Redux 来管理 React 的公共数据，但自从 React Hook 概念出现以后，可以使用 useContext + useRedux 轻松实现 Redux 相似功能。这一部分会在 “useReducer 高级用法” 中做详细讲解。

让我们回到 useContext 基础学习中。

## useReducer 是来解决什么问题的？

答：useReducer 是 useState 的升级版(实际上应该是原始版)，可以实现复杂逻辑修改，而不是像 useState 那样只是直接赋值修改。

补充说明：  
1、在 React 源码中，实际上 useState 就是由 useReducer 实现的，所以 useReducer 准确来说是 useState 的原始版。  
2、无论哪一个 Hook 函数，本质上都是通过事件驱动来实现视图层更新的。

## useReducer 函数源码：

回到 useReducer 的学习中，首先看一下 React 源码中的[ReactHooks.js](https://github.com/facebook/react/blob/master/packages/react/src/ReactHooks.js)。

    //备注：源码采用TypeScript编写，如果不懂TS代码，阅读起来稍显困难
    export function useReducer<S, I, A>(
      reducer: (S, A) => S,
      initialArg: I,
      init?: I => S,
    ): [S, Dispatch<A>] {
      const dispatcher = resolveDispatcher();
      return dispatcher.useReducer(reducer, initialArg, init);
    }

上述代码看不懂没关系，本系列教程只是讲述“如何使用 Hook”，并不是“Hook 源码分析”。之所以贴出源码只是想让你重点看一下 useReducer 函数的第 3 个参数。一般我们只传 2 个参数，如果有一天你看到有人为了某些不常用的目的传了 3 个参数，你应该理解，第 3 个参数其实只是第 1 和第 2 个参数的某种转化。事实上你可以完全忽略这个问题，每次值传 2 个参数即可。^\_^

## useReducer 基本用法

useReducer(reducer,initialValue)函数通常传入 2 个参数，第 1 个参数为我们定义的一个“由 dispatch 引发的数据修改处理函数”，第 2 个参数为自定义数据的默认值，useReducer 函数会返回自定义变量的引用和该自定义变量对应的“dispatch”。

请注意，当你看到了 dispatch，肯定想到了原生 JS 中的 EventEmitter，事实上 React Hook 帮我们做了底层的事件驱动处理，我们拿到的 dispatch 以及“事件处理函数”reducer，都时被 React Hook 封装过后的，并不是真正的抛出和事件处理函数。

但是为了更容易让你理解，本文依然会在讲解 useReducer 时使用到“事件抛出、事件处理函数”等文字。

如果你了解事件驱动，使用过 EventEmitter，或者你使用过 Redux，那么你会很容易理解 useReducer 的用法。

##### 代码形式：

    import React, { useReducer } from 'react'; //引入useReducer

    //定义好“事件处理函数” reducer
    function reducer(state, action) {
      switch (action) {
        case 'xx':
            return xxxx;
        case 'xx':
            return xxxx;
        default:
            return xxxx;
      }
    }

    function Component(){
      //声明一个变量xxx，以及对应修改xxx的dispatch
      //将事件处理函数reducer和默认值initialValue作为参数传递给useReducer
      const [xxx, dispatch] = useReducer(reducer, initialValue);

      //若想获取xxx的值，直接使用xxx即可

      //若想修改xxx的值，通过dispatch来修改
      dispatch('xx');
    }

    //请注意，上述代码中的action只是最基础的字符串形式，事实上action可以是多属性的object，这样可以自定义更多属性和更多参数值
    //例如 action 可以是 {type:'xx',param:xxx}

##### 拆解说明：

1、具体讲解已在上面示例代码中做了多项注释，此处不再重复；

#### 'reducer'补充说明

1、reducer 英文单词本身意思是“减速器、还原剂”，但是本文中一直把 reducer 称呼为“事件处理函数”，但事实上 reducer 确实扮演一个事件处理函数。  
2、千万不要把 useReducer 中的 reducer 和 原生 JS 中的 Array.prototype.reduce()弄混淆，他们两个只是刚好都使用了这个 reduce 单词而已，两者本身没有任何内在关联。

#### 'xxx'补充说明

假设我们定义的变量名为 xxx，那么只能通过 dispatch 来修改 xxx，不要尝试通过 xxx = newValue 这种形式直接修改变量的值，React 不允许这样做。

#### 'dispatch'补充说明

再次强调，dispacth 并不是真正的 Event.dispatch，但是你完全可以把它当成 Event.dispatch 来理解，只不过 useReducer 中的 dispacth(xxx)函数抛出内容不是 event，而是一个包含修改信息的对象，该对象不仅可以是字符串，还可以是复杂对象。

#### 'initialValue'补充说明

initialValue 是我们自定义变量的默认值，该值可以是简单类型(number、string)，也可以是复杂类型(object、array)。  
推荐建议：即使该值是简单类型，也建议单独定义出来而不是直接将值写在 useReducer 函数中，因为单独定义可以让我们更加清晰读懂数据结构，尤其是 initialValue 为复杂类型时。

## useReducer 使用示例 1：

举例：若某 React 组件内部有一个变量 count，默认值为 0，有 3 个 button，点击之后分别可以修改 count 的值。3 个按钮具体的功能为：第 1 个 button 点击之后 count+1，第 2 个 button 点击之后 count -1，第 3 个 button 点击之后 count x 2 (翻倍)。

若使用 useState 来实现，那肯定没问题，每个 button 点击之后分别运算得到对应的新值，将该值直接通过 setCount 赋予给 count。

若使用 useReducer 来实现相同功能，代码示例如下：

    import React, { useReducer } from 'react';

    function reducer(state,action){
      switch(action){
        case 'add':
            return state + 1;
        case 'sub':
            return state - 1;
        case 'mul':
            return state * 2;
        default:
            console.log('what?');
            return state;
      }
    }

    function CountComponent() {
      const [count, dispatch] = useReducer(reducer,0);

      return <div>
        {count}
        <button onClick={() => {dispatch('add')}} >add</button>
        <button onClick={() => {dispatch('sub')}} >sub</button>
        <button onClick={() => {dispatch('mul')}} >mul</button>
      </div>;
    }

    export default CountComponent;

代码分析：  
3 个按钮点击之后，不再具体去直接修改 count 的值，而是采用 dispatche('xxx')的形式 “抛出修改 count 的事件”，事件处理函数 reducer“捕获到修改 count 的事件后”，根据该事件携带的命令类型来进一步判断，并真正执行对 count 的修改。

请注意上面这句话中加引号的语句，本文只是以事件驱动的语言来描述整个过程，目的希望你能更加容易理解。

3 个按钮只是负责通知 reducer“我希望做什么事情”，具体怎么做完全由 reducer 来执行。这样实现了修改数据具体执行逻辑与按钮点击处理函数的抽离。

如果不使用 useReducer，而是使用之前学习过的 useState，那么对 count 的每一种修改逻辑代码，都必须分散写在每个按钮的点击事件处理函数中。

若只是修改 count 的功能，那么 useReducer 的优势还未全部体现出来，我们接着看另外一个示例。

## useReducer 使用示例 2

举例：在示例 1 中对 count 执行的修改，数值变动都是固定的，即 +1、-1、x 2。假设我们希望按钮点击之后，能够自主控制增加多少、减多少、或乘以几，这个效果该怎么实现呢？

很简单，我们将 dispatch('xxx')中的 xxx 由字符串改为 obj，obj 可以携带更多属性作为参数传给 reducer。 比如之前对 "加"的命令 dispatch('add')，修改为 dispatch({type:'add',param:2})。 reducer 可以通过 action.type 来区分是哪种命令、通过 action.param 来获取对应的参数。

为了简化代码，我们将在点击按钮后，随机产生一个数字，并将该数字作为 param 的值，传递给 reducer。

修改后的代码为：

    import React, { useReducer } from 'react';

    function reducer(state,action){
      //根据action.type来判断该执行哪种修改
      switch(action.type){
        case 'add':
          //count 最终加多少，取决于 action.param 的值
          return state + action.param;
        case 'sub':
          return state - action.param;
        case 'mul':
          return state * action.param;
        default:
          console.log('what?');
          return state;
      }
    }

    function getRandom(){
      return Math.floor(Math.random()*10);
    }

    function CountComponent() {
      const [count, dispatch] = useReducer(reducer,0);

      return <div>
        {count}
        <button onClick={() => {dispatch({type:'add',param:getRandom()})}} >add</button>
        <button onClick={() => {dispatch({type:'sub',param:getRandom()})}} >sub</button>
        <button onClick={() => {dispatch({type:'mul',param:getRandom()})}} >mul</button>
      </div>;
    }

    export default CountComponent;

同样的道理，我们可以把示例中的 count 由简单类型改为复杂类型，来储存更多的变量。 但是，建议不要把 useReducer 对应的变量设计的过于复杂。

使用 useReducer，可以让我们使用比较复杂的逻辑和参数对内部变量进行修改。

不过你是否发现，示例 1 和示例 2 中所有的变量都是在同一个组件内定义和修改的，现实项目中肯定牵扯到不同模块组件之间共享并修改某个变量，那又该怎么办呢？  
在下一章节 useReducer 高级用法 中，我们会详细讲述如何用 useReducer + useContext 来实现全局不同层级组件共享并修改某变量。

---

至此，关于 useReducer 基础用法已经讲完。
