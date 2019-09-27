# React-Redux-Reference
A quick reference guide to help new React-Redux users

Concepts to be covered:
Provider, connect, mapStateToProps, mapDispatchToProps, action creators

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

**Pop Quiz: is `mapStateToProps` a callback function?**

You can declare it using the `function` keyword or as an arrow
function, it doesn't matter. The first argument `mapStateToProps` takes
is the entire *store* state (often `state` or `reduxState`). Since we declare this
function within the component file I find it least confusing to call the
argument `reduxState` so that you don't confuse it with component state.

`mapStateToProps` should return an object. This object should declare what
properties we want to add to props on our component and have accurate corresponding
values.

```
function mapStateToProps(reduxState){
  return {
    user: reduxState.user
  }
}

export default connect(
  mapStateToProps
)(MyComponent)
```

**Note: if you don't need any values from redux state on props and just want a component to
be able to dispatch actions you can pass `null` in place of `mapStateToProps` and then pass
your `mapDispatchToProps`.**

```
export default connect(
  null,
  mapDispatchToProps
)(MyComponent)
```



### `mapDispatchToProps`

`mapDispatchToProps` can take two forms: a function or an object. 
The React-Redux docs recommend using the object form so we will focus 
on it. Know that either way - function or object form - results in an object
with multiple methods being passed into connect. These methods are the action
creators that you have likely declared within a separate `actionCreators.js`
file or within your `reducer.js`.

You should import your action creators at the top of the file and then package
them into an object that you pass as the second argument to `connect()`.

```
import { actionCreator1, actionCreator2, ... } from "../dux/someReducer"

...

export default connect(
  mapStateToProps,
  { actionCreator1, actionCreator2, ... }
)(MyComponent)
```

You might also see it noted like this:

```
import { actionCreator1, actionCreator2, ... } from "../dux/someReducer"

...

const mapDispatchToProps = {
  actionCreator1,
  actionCreator2,
  ...
}

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(MyComponent)
```

This results in adding your action creators to the `props` object so that you
can invoke them like `this.props.actionCreator1()` within the connected component.

### `Action Creators`

Action Creators are not necessarily specific to React-Redux but we
introduced them with it so I will cover them here. Action creators are
simply the functions that we dispatch to the store. You'll recall that
in plain Redux we did something like this:

```
store.dispatch({
  type: SOME_TYPE,
  payload: someValueHere
})
```

Our action creators are essentially just assigning this to a function.

```
function actionCreator1(){
  return {
    type: SOME_TYPE,
    payload: someValueHere
  }
}
```

You'll notice that our function returns an object identical to the object
we passed into `dispatch`. But where does dispatch come in? When we use the
object shorthand form of `mapDispatchToProps`, connect will automatically bind
our action creators to the dispatch function call so that we can invoke our action
creators directly without calling `dispatch()`.

**We usually declare these functions in the `reducer.js` that they will
affect or in a separate `actionCreators.js` file for good organization.**


**Answer to pop quiz: Yes. Remember, a callback function is simply a function that
is passed into another function as an argument**

#### For more information visit the [docs](https://react-redux.js.org/introduction/quick-start)! 