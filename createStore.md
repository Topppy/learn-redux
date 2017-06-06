```
import isPlainObject from 'lodash/isPlainObject'
```
> Import a function to checks if `argument` is a plain object, that is, an object created by the * `Object` constructor or one with a  `[[Prototype]]` of `null`. 

```
/**
 * These are private action types reserved by Redux.
 * For any unknown actions, you must return the current state.
 * If the current state is undefined, you must return the initial state.
 * Do not reference these action types directly in your code.
 */
export var ActionTypes = {
  INIT: '@@redux/INIT'
}
```
ActionTypes is a 'action' in redux. it is used at the end of function **createStore**.



```
export default function createStore(reducer, preloadedState, enhancer) {
  if (typeof preloadedState === 'function' && typeof enhancer === 'undefined') {
    enhancer = preloadedState
    preloadedState = undefined
  }
```

If there are only two arguments passed in and the type of second one is *Function*, excange the value between **preloadState** and **enhancer**.



```
if (typeof enhancer !== 'undefined') {
    if (typeof enhancer !== 'function') {
      throw new Error('Expected the enhancer to be a function.')
    }

    return enhancer(createStore)(reducer, preloadedState)
  }
```
if enhancer passed in isn't *Function*, tell the user "Error". when the enhancer function is correctly passed in (*The only store enhancer that ships with Redux is `applyMiddleware()`.*), give all the things to enhancer.Also, we can use `applyMiddleware()` in such a way :`applyMiddleware(createStore)(reducer, preloadedState)`.
```
if (typeof reducer !== 'function') {
    throw new Error('Expected the reducer to be a function.')
  }
```
Make sure reducer is function.
```
  var currentReducer = reducer
  var currentState = preloadedState
  var currentListeners = []
  var nextListeners = currentListeners
  var isDispatching = false
  
  function ensureCanMutateNextListeners() {
    if (nextListeners === currentListeners) {
      nextListeners = currentListeners.slice()
    }
  }
```
*ensureCanMutateNextListeners* ensure nextListeners and curretListeners different.
```
function getState() {
    return currentState
  }
```
Getter of store ,return the current state tree.

```
  function subscribe(listener) {
    if (typeof listener !== 'function') {
      throw new Error('Expected listener to be a function.')
    }

    var isSubscribed = true

    ensureCanMutateNextListeners()
    nextListeners.push(listener)

    return function unsubscribe() {
      if (!isSubscribed) {
        return
      }

      isSubscribed = false

      ensureCanMutateNextListeners()
      var index = nextListeners.indexOf(listener)
      nextListeners.splice(index, 1)
    }
  }
  ```








