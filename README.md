# React-Redux-Reference
A quick reference guide to help new React-Redux users


connect, mapStateToProps, mapDispatchToProps, actions, action creators, Provider

### `Provider`

React Redux provides `<Provider />`, which determines the Redux `store` available
to all components that it wraps. The `store` prop is necessary and is really
what dictates the `store` that components will access.

```
<Provider store={store}>
  <App />
</Provider>
```

### `connect()`

`connect` is a function that actually *connects* your component to the store that
we assigned in `<Provider>`. It accepts two parameters, mapStateToProps
and mapDispatchToProps (we'll address these next). `connect()` returns a higher
order component (HOC) that wraps your component to give it access to the 
props we want it to get from `Redux`. We'll go over HOC's later in the course.
It usually looks like this:

```
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(MyComponent)
```

It may help to see what is going on more broken out like so:

```
const returnedHOC = connect(mapStateToProps, mapDispatchToProps)

const componentWithPropsFromRedux = returnedHOC(MyComponent)

export default componentWithPropsFromRedux;
```