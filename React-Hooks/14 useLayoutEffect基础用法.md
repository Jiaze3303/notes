# 14 useLayoutEffect 基础用法

## useLayoutEffect 概念解释

我们第九个要学习的 Hook(钩子函数)是 useLayoutEffect，他的作用是“勾住”挂载或重新渲染完成这 2 个组件生命周期函数。useLayoutEffect 使用方法、所传参数和 useEffect 完全相同。

他们的不同点在于，你可以把 useLayoutEffect 等同于 componentDidMount、componentDidUpdate，因为他们调用阶段是相同的。而 useEffect 是在 componentDidMount、componentDidUpdate 调用之后才会触发的。

也就是说，当组件所有 DOM 都渲染完成后，同步调用 useLayoutEffect，然后再调用 useEffect。

useLayoutEffect 永远要比 useEffect 先触发完成。

那通常在 useLayoutEffect 阶段我们可以做什么呢？  
答：在触发 useLayoutEffect 阶段时，页面全部 DOM 已经渲染完成，此时可以获取当前页面所有信息，包括页面显示布局等，你可以根据需求修改调整页面。

请注意，useLayoutEffect 对页面的某些修改调整可能会触发组件重新渲染。如果是对 DOM 进行一些样式调整是不会触发重新渲染的，这点和 useEffect 是相同的。

在 react 官方文档中，明确表示只有在 useEffect 不能满足你组件需求的情况下，才应该考虑使用 useLayoutEffect。 官方推荐优先使用 useEffect。

请注意：如果是服务器渲染，无论 useEffect 还是 useLayoutEffect 都无法在 JS 代码加载完成之前执行，因此都会收到错误警告。 服务器渲染时若想使用 useEffect，解决方案不在本章中讨论。

让我们回到 useLayoutEffect 基础学习中。

## useLayoutEffect 是来解决什么问题的？

答：useLayoutEffect 的作用是“当页面挂载或渲染完成时，再给你一次机会对页面进行修改”。

如果你选择使用 useLayoutEffect，对页面进行了修改，更改样式不会引发重新渲染，但是修改变量则会触发再次渲染。  
如果你不使用 useLayoutEffect，那么之后就应该调用 useEffect。

补充说明：  
1、优先使用 useEffect，useEffect 无法满足需求时再考虑使用 useLayoutEffect。  
2、useLayoutEffect 先触发，useEffect 后触发。  
3、useEffect 和 useLayoutEffect 在服务器端渲染时，都不行，需要寻求别的解决方案。

## useLayoutEffect 函数源码：

回到 useLayoutEffect 的学习中，首先看一下 React 源码中的[ReactHooks.js](https://github.com/facebook/react/blob/master/packages/react/src/ReactHooks.js)。

    //备注：源码采用TypeScript编写，如果不懂TS代码，阅读起来稍显困难
    export function useLayoutEffect(
      create: () => (() => void) | void,
      deps: Array<mixed> | void | null,
    ): void {
      const dispatcher = resolveDispatcher();
      return dispatcher.useLayoutEffect(create, deps);
    }

上述代码看不懂没关系，本系列教程只是讲述“如何使用 Hook”，并不是“Hook 源码分析”。^\_^ 你只需知道 useLayoutEffect 的用法和 useEffect 一模一样即可。

## useLayoutEffect 基本用法

useLayoutEffect 的用法和 useEffect 的用法相同，所以不再阐述。

## useLayoutEffect 使用示例：

请原谅，目前竟然找不到一个 useLayoutEffect 合适的例子，因为能够想到的应用场景其实都可以用 useEffect 来代替。

那只能贴出一段简单的代码，让你看确认一下，useLayoutEffect 先于 useEffect 触发调用。

代码示例如下：

    import React,{useState,useEffect,useLayoutEffect} from 'react'

    function LayoutEffect() {
      const [count,setCount] = useState(0);

      useEffect(() => {
        console.log('useEffect...');
      },[count]);

      useLayoutEffect(() => {
        console.log('useLayoutEffect...');
      },[count]);

      return (
        <div>
            {count}
            <button onClick={() => {setCount(count+1)}}>Click</button>
        </div>
      )
    }
    export default LayoutEffect

实际运行就会发现：  
无论是首次挂载，还是重新渲染，console 面板中，输出顺序都是  
useLayoutEffect...  
useEffect...

也就确认，先执行 useLayoutEffect，后执行 useEffect。

---

至此，关于 useLayoutEffect 基础用法已经讲完，没有高级用法，直接进入下一个 Hook。
