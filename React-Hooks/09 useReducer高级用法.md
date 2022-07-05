# 09 useReducer 高级用法

所谓高级用法，只不过是一些深层知识点和实用技巧，你甚至可以把本章当做对前面知识点的一个巩固和学习。

<br>

## 使用 useReducer 来管理复杂类型的数据

举例，若某组件内通过 ajax 请求数据，获取最新一条站内短信文字，需要组件显示整个 ajax 过程及结果：  
1、当 ajax 开始请求时，界面显示“loading...”；  
2、当 ajax 请求发生错误时，界面显示“wrong!”;  
3、当 ajax 请求成功获取数据时，界面显示获取到的数据内容；

如果我们使用 useState 来实现上述功能，伪代码如下：

    function Component() {
      const [loading,setLoading] = useState(true); //是否ajax请求中，默认为true
      const [result,setResult] = useState(''); //请求数据内容，默认为''
      const [error,setError] = useState(false); //请求是否发生错误，默认为false

      {
          //ajax请求成功
          setLoading(false);
          setResult('You have a good news!');//请注意，这是一行伪代码，只是为了演示，并不是真正ajax获取的结果
          setError(false);

          //ajax请求错误
          setLoading(false);
          setError(true);
      }

      return <div>
        {loading ? 'loading...' : result}
        {error ? 'wrong!' : null}
      </div>
    }

如果我们使用 useReducer 来实现，则可将上述 3 个变量都放在我们定义的变量 state 中，伪代码如下：

    const initralData = {loading: true,result: '',error: false};

    const reducer = (state, action) => {
      switch (action.type) {
        case 'succes':
            return {loading:false,result:action.res,error:false}
        case 'error':
            return {loading:false,error:true}
      }
    }

    function Component() {
      const [state, dispatch] = useReducer(reducer, initralData);

      {
          //ajax请求成功
          dispatch({type:'succes',res:'You have a good news!'});

          //ajax请求错误
          dispatch({type:'error'});
      }

      return <div>
        {state.loading ? 'loading...' : state.result}
        {state.error ? 'wrong!' : null}
      </div>
    }

你可能会有疑问？  
1、为什么看上去使用 useReducer 后代码变得更多？  
答：因为使用 useReducer，我们将修改数据拆分为 2 个部分，即“抛出修改事件和事件修改处理函数”。虽然代码增多了，但是逻辑更加清晰。

2、为什么不使用 useState，同时把它对应的变量也做成一个 obj，就像 useReducer 的 initralData 那种？  
答：单纯从 1 次 ajax 请求很难看出使用 useState 或 useReducer 的差异，但是试想一下多次且 ajax 返回值在结构类型上容易发生变更，那么使用 useReducer 这种更加利于代码阅读、功能扩展。

<br>

## 使用 useContext 和 useReducer 实现操作全局共享数据

试想一下，如果想实现以下组件需求：  
1、父组件中定义某变量 xx；  
2、任何层级下的子组件都可以轻松获取变量 xx、并且可以“修改”变量 xx；

注意这里的修改是加引号的，因为事实上你永远无法以直接赋值的方式进行修改，永远都需要调用父级组件提供的方法来修改。

#### 需求分析

激动的心，颤抖的手，忘掉 Redux，拥抱 React Hook！

首先这个功能是类组件无法做到的，也是 React 16.8 版本以前根本不能实现的，今天，当你使用 Hook 可轻松实现类似 Redux 共享数据管理功能。

#### 实现原理

用 useContext 实现“获取全局数据”  
用 useReducer 实现“修改全局数据”

#### 实现思路

1、用 React.createContext()定义一个全局数据对象；  
2、在父组件中用 useReducer 定义全局变量 xx 和负责抛出修改事件的 dispatch；  
3、在父组件之外，定义负责具体修改全局变量的处理函数 reducer，根据修改 xx 事件类型和参数，执行修改 xx 的值；  
4、在父组件中用 <XxxContext.Provider value={{xx,dispathc}}> 标签把 全局共享数据和负责抛出修改 xx 的 dispatch 暴露给子组件；  
5、在子组件中用 useContext 获取全局变量；  
6、在子组件中用 xxContext.dispatch 去抛出修改 xx 的事件，携带修改事件类型和参数；

#### 补充说明

上面一直提到了 “抛出事件” “事件处理函数” "dispacth" 都是字面上的，不是真正意义上的事件驱动。 这些都只是 React 暴露给我们的函数或形参。 真正的事件驱动是由 React Hook 底层为我们完成的。

以上观点仅为个人理解，不能保证 100%正确。

#### 伪代码演示

假设 React 组件需求为：  
1、有全局数据变量 count；  
2、不同层级的子组件均可获取并修改全局变量 count；

共享对象 代码如下：

    import React from 'react';
    const CountContext = React.createContext();
    export default CountContext;

父组件 代码如下：

    import React, { useReducer } from 'react';
    import CountContext from './CountContext';
    import ComponentA from './ComponentA';
    import ComponentB from './ComponentB';
    import ComponentC from './ComponentC';

    const initialCount = 0; //定义count的默认值

    //修改count事件处理函数，根据修改参数进行处理
    function reducer(state, action) {
    //注意这里先判断事件类型，然后结合携带的参数param 来最终修改count
    switch (action.type) {
        case 'add':
            return state + action.param;
        case 'sub':
            return state - action.param;
        case 'mul':
            return state * action.param;
        case 'reset':
            return initialCount;
        default:
            console.log('what?');
            return state;
    }
    }

    function ParentComponent() {
      //定义全局变量count，以及负责抛出修改事件的dispatch
      const [count, dispatch] = useReducer(reducer, initialCount);

      //请注意：value={{count,dispatch} 是整个代码的核心，把将count、dispatch暴露给所有子组件
      return <CountContext.Provider value={{count,dispatch}}>
        <div>
            ParentComponent - count={count}
            <ComponentA />
            <ComponentB />
            <ComponentC />
        </div>
      </CountContext.Provider>
    }

    export default ParentComponent;

子组件 A 代码如下：

    import React,{ useState, useContext } from 'react';
    import CountContext from './CountContext';

    function CopmpoentA() {
      const [param,setParam] = useState(1);
      //引入全局共享对象，获取全局变量count，以及修改count对应的dispatch
      const countContext = useContext(CountContext);

      const inputChangeHandler = (eve) => {
        setParam(eve.target.value);
      }

      const doHandler = () => {
        //若想修改全局count，先获取count对应的修改抛出事件对象dispatch，然后通过dispatch将修改内容抛出
        //抛出的修改内容为：{type:'add',param:xxx}，即告诉count的修改事件处理函数，本次修改的类型为add，参数是param
        //这里的add和param完全是根据自己实际需求自己定义的
        countContext.dispatch({type:'add',param:Number(param)});
      }

      const resetHandler = () => {
        countContext.dispatch({type:'reset'});
      }

      return <div>
            ComponentA - count={countContext.count}
            <input type='number' value={param} onChange={inputChangeHandler} />
            <button onClick={doHandler}>add {param}</button>
            <button onClick={resetHandler}>reset</button>
        </div>
    }

    export default CopmpoentA;

总结：  
1、3 个子组件他们主要区别是组件内 doHandler 函数，对 count 进行不同形式的修改；  
2、3 个子组件分别可以实现对全局变量 count 的获取与修改；  
3、当任何一个子组件对 count 进行了修改，都会立即反映在其他子组件中，实现子组件之间的数据共享。

至此，实现了比较简单的，类似 Redux 全局数据管理效果。

<br>

## 为什么不使用 Redux？

这个问题以前提出过，现在可以明确回答：因为我自己使用 useReducer + useContext 自己可以轻松实现，干嘛还要用 Redux。  
再见 Redux。

<br>

<br>

> 以下内容更新于 2021.05.18

## 忘掉 Redux，忘掉 useReducer+useContext，拥抱 Recoil 吧！

强烈推荐使用 React 开发人员针对 Hooks 函数组件推出的新一代状态管理库：Recoil

Recoil 官方网站：https://recoiljs.org/

我写的 Recoil 教程：https://github.com/puxiao/recoil-tutorial

> 以上内容更新于 2021.05.18

<br>

<br>

## 什么时候用 useState？什么时候用 useReducer？

本人的建议是：组件自己内部的简单逻辑变量用 useState、多个组件之间共享的复杂逻辑变量用 useReducer。

---

至此，关于 useReducer 高级用法已经讲完，useReducer 可以让我们实现复杂逻辑的数据修改，结合 useContext 更能做到全局数据共享和修改。

目前已经学习过的 4 个 Hook 函数 useState、useEffect、useContext、useReducer，他们都是用来实现组件某些具体业务功能的，而接下来要学习的 Hook 函数则是用来提高组件整体性能的，例如第 5 个 Hook 函数 useCallback 和第 6 个 Hook 函数 useMemo。
