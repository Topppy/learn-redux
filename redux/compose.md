## source code from ```redux/src/compose.js```
```
/**
 * Composes single-argument functions from right to left. The rightmost
 * function can take multiple arguments as it provides the signature for
 * the resulting composite function.
 *
 * @param {...Function} funcs The functions to compose.
 * @returns {Function} A function obtained by composing the argument functions
 * from right to left. For example, compose(f, g, h) is identical to doing
 * (...args) => f(g(h(...args))).
 */

export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  const last = funcs[funcs.length - 1]
  const rest = funcs.slice(0, -1)
  return (...args) => rest.reduceRight((composed, f) => f(composed), last(...args))
}
```
## What it is doing
```
export default function compose(...funcs) {
```
Several es6 features is used in this line , it means a function named *compose* exported as default function from this module. you can import the *compose* function in other mudules by ```import compose from './compose'``` which you find in file ```redux/src/applyMiddleware.js```. The expression in brackets after function name *compose* using another new feature: *rest Parameters*, an amount of arguments we dont kown how many they are is passed in this function, we can get them in Array ```funcs```.

```
if (funcs.length === 0) {
    return arg => arg
  }
```

this condition means if there is no function passing in as argumnet, function compose will return a function which outputs whatever it gets.

```
  if (funcs.length === 1) {
    return funcs[0]
  }
```
if there is only one function is passing in, just return it.
```
const last = funcs[funcs.length - 1]
```
Save the last function in Array funcs as variable ```last```.
```
const rest = funcs.slice(0, -1)
```
Save the rest functions except the last one as variable ```rest```.
```
return (...args) => rest.reduceRight((composed, f) => f(composed), last(...args))
```
Here is the importent code in this module.it may looks a little hard to understand for those who is unfamiliar to ES6,we transform them to ES5.
```
return function (...args){
  return rest.reduceRight(function(composed,f){
    f(composed)
  }, last(...args))
}
```
It returns a function accepted arguments as normal which is easy to use.Inside this returned function, arguments are passed in functions of Array *funcs* in the order from right to left.For example, if ```funcs = [a,b,c]```, it returns a compound function ```a(b(c(args)))``` 
