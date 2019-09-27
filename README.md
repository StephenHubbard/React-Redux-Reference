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

It may help to see what is going on more broken out, like so:

```
// connect returns a function (also called a HOC)
const returnedHOC = connect(mapStateToProps, mapDispatchToProps)

// we then invoke that function (HOC), passing in our component
const componentWithPropsFromRedux = returnedHOC(MyComponent)

// finally, we export the resulting component from our file
export default componentWithPropsFromRedux;
```

### `mapStateToProps`

`mapStateToProps` is a function and the first parameter that `connect()`
accepts. 

**Pop Quiz #1: is `mapStateToProps` a callback function?**

You can declare it using the `function` keyword or as an arrow
function, it doesn't matter. The first argument `mapStateToProps` takes
is the entire store state (often state or reduxState). Since we declare this
function within the component file I find it least confusing to call the
argument reduxState so that you can more easily reason through it.

`mapStateToProps` should return an object. This object should declare what
properties we want to add to props on our component and have accurate corresponding
values.

```
function mapStateToProps(reduxState){
  return {
    user: reduxState.user
  }
}
```

### `mapDispatchToProps`

