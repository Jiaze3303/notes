React 还有一个设置默认 props 的选项。 可以将默认 props 作为组件本身的属性分配给组件，React 会在必要时分配默认 prop。 如果没有显式的提供任何值，这允许指定 prop 值应该是什么。 例如，如果声明 `MyComponent.defaultProps = { location: 'San Francisco' }`，即定义一个 location 属性，并且其值在没有另行制定的情况下被设置为字符串 `San Francisco`。 如果 props 未定义，则 React 会分配默认 props，但如果你将 `null` 作为 prop 的值，它将保持 `null`。


