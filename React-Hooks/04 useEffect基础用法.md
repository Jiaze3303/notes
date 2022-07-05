# 04 useEffect 基础用法

## useEffect 概念解释

我们第二个要学习的 Hook(钩子函数)是 useEffect，他的作用是“勾住”函数组件中某些生命周期函数。

都能勾住哪些生命周期函数？  
答：componentDidMount(组件被挂载完成后)、componentDidUpdate(组件重新渲染完成后)、componentWillUnmount(组件即将被卸载前)

为什么是这 3 个生命周期函数？  
答：因为修改数据我们可以使用前面学到的 useState，数据变更会触发组件重新渲染，上面 3 个就是和组件渲染关联最紧密的生命周期函数。

那其他生命周期函数呢？  
答：该问题的回答，引用[React 官方中文文档 FAQ](https://react.docschina.org/docs/hooks-faq.html#do-hooks-cover-all-use-cases-for-classes)，如下

> 我们给 Hook 设定的目标是尽早覆盖 class 的所有使用场景。目前暂时还没有对应不常用的 getSnapshotBeforeUpdate，getDerivedStateFromError 和 componentDidCatch 生命周期的 Hook 等价写法，但我们计划尽早把它们加进来。

## useEffect 是来解决类组件什么问题的？

答：useEffect 是来解决类组件 **某些执行代码被分散在不同的生命周期函数中** 的问题。

举例 1：若某类组件中有变量 a，默认值为 0，当组件第一次被挂载后或组件重新渲染后，将网页标题显示为 a 的值。  
那么在类组件里，我们需要写的代码是：

    //为了更加清楚看到每次渲染，我们在网页标题中 a 的后面再增加一个随机数字
    componentDidMount(){
        document.title = `${this.state.a} - ${Math.floor(Math.random()*100)}`;
    }
    componentDidUpdate(){
        document.title = `${this.state.a} - ${Math.floor(Math.random()*100)}`;
    }

从上面这种代码里你会看到，为了保证第一次被挂载、组件重新渲染后都执行修改网页标题的行为，相同的代码我们需要分别在 componentDidMount、componentDidUpdate 中写 2 次。

举例 2：假设需要给上面那个组件新增一个功能，当组件第一次被挂载后执行一个自动累加器 setInterval，每 1 秒 a 的值+1。为了防止内存泄露，我们在该组件即将被卸载前清除掉该累加器。  
那么在类组件里，我们需要写的代码是：

    timer = null;//新增一个可内部访问的累加器变量(注：类组件定义属性时前面无法使用 var/let/const)
    componentDidMount(){
        document.title = `${this.state.a} - ${Math.floor(Math.random()*100)}`;
        this.timer = setInterval(() => {this.setState({a:this.state.a+1})}, 1000);//添加累加器
    }
    componentDidUpdate(){
        document.title = `${this.state.a} - ${Math.floor(Math.random()*100)}`;
    }
    componentWillUnmount(){
        clearInterval(this.timer);//清除累加器
    }

从上面代码可以看到，增加累加器和清除累加器这 2 个相关的执行代码被分别定义在 componentDidMount、componentWillUnmount 这两个生命周期函数中。

举例 3：假设给上面的组件再新增一个变量 b，当 b 的值发生变化后也会引发组件重新渲染，然后呢？有什么隐患吗？  
答：b 的值改变引发组件重新渲染，然后肯定是会触发 componentDidUpdate 函数，这时会让修改网页标题的代码再次执行一次，尽管此时 a 的值并没有发生任何变化。

再来回顾一下上面的 3 个例子：  
1、举例 1 中，相同的代码可能需要在不同生命周期函数中写 2 次；  
2、举例 2 中，相关的代码可能需要在不同生命周期函数中定义；  
3、举例 3 中，无论是哪个原因引发的组件重新渲染，都会触发生命周期函数的执行，造成一些不必要的代码执行；

以上就是 类组件“某些执行代码被分散在不同的生命周期函数中”引发的问题具体表现，而 useEffect 就是来解决这些问题的。

接下来开始学习 useState。

## useEffect 函数源码：

回到 useEffect 的学习中，首先看一下 React 源码中的[ReactHooks.js](https://github.com/facebook/react/blob/master/packages/react/src/ReactHooks.js)。

    //备注：源码采用TypeScript编写，如果不懂TS代码，阅读起来稍显困难
    export function useEffect(
      create: () => (() => void) | void,
      deps: Array<mixed> | void | null,
    ): void {
      const dispatcher = resolveDispatcher();
      return dispatcher.useEffect(create, deps);
    }

上述代码看不懂没关系，本系列教程只是讲述“如何使用 Hook”，并不是“Hook 源码分析”。之所以贴出源码只是为了让你以后也可以给面试官吹嘘你读过 React 源码。^\_^

## useEffect 基本用法

useEffect(effect,[deps])函数可以传入 2 个参数，第 1 个参数为我们定义的执行函数、第 2 个参数是依赖关系(可选参数)。若一个函数组件中定义了多个 useEffect，那么他们实际执行顺序是按照在代码中定义的先后顺序来执行的。

具体说明如下：  
第 1 个值 effect 是一个 function，用来编写 useEffect 对应的执行代码。  
还记得本文开头提到的 useEffect 能勾住哪 3 个生命周期函数吗？  
componentDidMount、componentDidUpdate、componentWillUnmount ，当上述 3 个生命周期函数执行后，就会触发 useEffect 函数，进而执行而第 1 个参数 effect 中的内容。

组件挂载后(componentDidMount)与组件重新渲染后(componentDidUpdate)对应的代码合并为一个函数这个容易理解，可是组件卸载前(componentWillUnmount)也能融入进来？  
答：是的，通过在 effect 中 return 一个函数来实现的。

关于第 2 个参数 [deps] ，先知道这个是可选参数，是 Hook 用来向 React 表明 useEffect 依赖关系的即可。关于它的用法会在 useEffect 高级用法中有更多详细讲述。

##### 代码形式：

    useEffect(() => {
        //此处编写 组件挂载之后和组件重新渲染之后执行的代码
        ...

        return () => {
            //此处编写 组件即将被卸载前执行的代码
            ...
        }
    },[deps])

之前说过 useEffect 第 1 个参数 effect 是个 function，只是这个 function 稍显复杂。

##### 拆解说明：

1、effect 函数主体内容中的代码，就是组件挂载之后和组件重新渲染之后你需要执行的代码；  
2、effect 函数 return 出去的返回函数主体内容中的代码，就是组件即将被卸载前你需要执行的代码；  
3、第 2 个参数 [deps]，为可选参数，若有值则向 React 表明该 useEffect 是依赖哪些变量发生改变而触发的；

#### 'effect'补充说明

1、若你不需要在组件卸载前执行任何代码，那么可以忽略不写 effect 中的 return 相关代码；

##### '[deps]'补充说明：

1、若缺省，则组件挂载、组件重新渲染、组件即将被卸载前，每一次都会触发该 useEffect；  
3、若传值，则必须为数组，数组的内容是函数组件中通过 useState 自定义的变量或者是父组件传值过来的 props 中的变量，告诉 React 只有数组内的变量发生变化时才会触发 useEffect；  
4、若传值，但是传的是空数组 []，则表示该 useEffect 里的内容仅会在“挂载完成后和组件即将被卸载前”执行一次；

## useEffect 使用示例：

还记得本文上面关于 类组件“某些执行代码被分散在不同的生命周期函数中”引发的问题时，所举的 3 个例子吗？  
我们用 Hook 来依次分别实现举例 1、举例 2、举例 3，通过 3 个功能的代码示例，让你明白 useEffect 的具体用法。

举例 1：若某类组件中有变量 a，默认值为 0，当组件第一次被挂载后或组件重新渲染后，将网页标题显示为 a 的值。  
补充说明：  
1、为了让 a 的值可以发生变化，我们在组件中添加一个按钮，每次点击 a 的值 +1  
2、为了更加清楚看到每次渲染，我们在网页标题中 a 的后面再增加一个随机数字

    import React, { useState,useEffect} from 'react';

    function Component() {
      const [a, setA] = useState(0);//定义变量a，并且默认值为0
      useEffect(() => {
          //无论是第一次挂载还是以后每次组件更新，修改网页标题的执行代码只需要在这里写一次即可
          document.title = `${a} - ${Math.floor(Math.random()*100)}`;
      })
      const clickAbtHandler = (eve) =>{
          setA(a+1);
      }
      return <div>
          {a}
          <button onClick={clickAbtHandler}>a+1</button>
        </div>
    }

    export default Component;

从上述代码可以看出，“类组件中相同的代码可能需要在不同生命周期函数中写 2 次”这个问题已通过 Hook useEffect 已解决。

这里只是实现列 举例 1 中的功能，是 useEffect 最基础的用法。举例 2、举例 3 中的功能实现我们放到 useEffect 高级用法 中来讲解。

---

至此，关于 useEffect 基础用法已经讲完。
