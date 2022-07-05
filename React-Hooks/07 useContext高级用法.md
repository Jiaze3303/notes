# 07 useContext 高级用法

所谓高级用法，只不过是一些深层知识点和实用技巧，你甚至可以把本章当做对前面知识点的一个巩固和学习。

## 同时传递多个共享数据值给 1 个子组件

实现以下组件需求：  
1、有 2 个共享数据对象 UserContext、NewsContext；  
2、父组件为 AppComponent、子组件为 ChildComponent；  
3、父组件需要同时将 UserContext、NewsContext 的数据同时传递给子组件；

实现代码：

    import React,{ useContext } from 'react'

    const UserContext = React.createContext();
    const NewsContext = React.createContext();

    function AppComponent() {
      return (
        <UserContext.Provider value={{name:'puxiao'}}>
            <NewsContext.Provider value={{title:'Hello React Hook.'}}>
                <ChildComponent />
            </NewsContext.Provider>
        </UserContext.Provider>
      )
    }

    function ChildComponent(){
      const user = useContext(UserContext);
      const news = useContext(NewsContext);
      return <div>
        {user.name} - {news.title}
      </div>
    }

    export default AppComponent;

代码分析：  
1、父组件同时要实现传递 2 个共享数据对象 value 值，需要使用<XxxContext.Provider value={obj}>标签进行 2 次嵌套。  
2、子组件使用了 useContext，他可以自由随意使用父组件传递过来的共享数据 value，并不需要多次嵌套获取。

## 同时将 1 个共享数据值传递给多个子组件

使用<XxxContext.Provider></XxxContext.Provider>标签将多个子组件包裹起来，即可实现。

    <XxxContext.Provider value={{name:'puxiao'}}>
        <ComponentA />
        <ComponentB />
        <ComponentC />
    </XxxContext.Provider>

3 个子组件<ComponentA /\>、<ComponentB /\>、<ComponentC /\>都可使用 useContext 获取共享数据值。

## 为什么不使用 Redux？

在 Hook 出现以前，React 主要负责视图层的渲染，并不负责组件数据状态管理，所以才有了第三方 Redux 模块，专门来负责 React 的数据管理。

但是自从有了 Hook 后，使用 React Hook 进行函数组件开发，实现数据状态管理变得切实可行。只要根据实际项目需求，使用 useContext 以及下一章节要学习的 useReducer，一定程度上是可以满足常见需求的。

毕竟使用 Redux 会增大项目复杂度，此外还要花费学习 Redux 成本。

具体需求具体分析，不必过分追求 Redux。

---

至此，关于 useContext 高级用法已经讲完，useContext 降低了组件之间数据传递的复杂性，让我们编写代码更加心情愉悦，而不用去关心层层嵌套问题。

接下来学习第 4 个 Hook 函数 useReducer。
