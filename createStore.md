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
ActionTypes is a 'action' in redux. it is used at the end of **createStore**












