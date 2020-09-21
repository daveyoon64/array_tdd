# Array.prototype.some() Part 1

## Definition
some() method checks entire array to see if at least one element passes the callback's condition 

## Example
```javascript
const array = [1, 2, 3, 4, 5];
// checks whether an element is even
const even = (element) => element % 2 === 0;
console.log(array.some(even));
// expected output: true
```

## Syntax
arr.some(callback(element[, index[, array]])[, thisArg])

## Parameters
callback(element being processed, index of current element, original array)

## Return
Returns true if at least one element passes
Else, returns false

## Description
- executes callback for each element in array until it finds a truthy value, immediately returns true
- if it doesn't find a truthy value in the element, it returns false
- the callback is invoked only for indexes with assigned values (holes, undefined)
    - check for all falsys? 0, null, undefined, '', NaN, false
- the callback is not invoked for deleted values
- it takes an optional this that can be bound to the callback's this
- some() does  not mutate the array
- elements appended to array while some() runs will not be visited
- if existing, unvisited element is mutated by callback, the updated value will be used
- deleted elements will not be visited


# Version 2
## Prototype Implementation
```javascript
function some(array, callback) {
  var isArrayTrue = false;
  for (var i = 0; i < array.length; i++) {
    isArrayTrue = callback(array[i]);
    if (isArrayTrue) { return isArrayTrue }; 
  }
  return isArrayTrue;
}
```

## My function declaration
some(array, callback, optionalThis)

## Callback parameters
element
currentIndex
originalArray

## Returns 
Returns true if at least one element passes. Else, it returns false

## Description
- run callback for each element until is returns a truthy, then return true
```javascript
some([1, 2, 3], function(number) {
  return number > 2;
})
// returns true
```
- run callback for each element. if no truthy value found, return false
```javascript
some([1, 2, 3], function(number) {
  return number > 4;
})
// returns false
```
- callback is invoked only for indexes with assigned values
```javascript
some([,,,,,4,,,32,,undefined], function(number) {
  return number > 30;
})
// returns true
```
- elements appended to array will not be visited
```javascript
var testArray = [1, 2, 3];
some(testArray, function(number) {
  testArray.push(20);
  return number < 10;
})
// returns true, since the 20 is never evaluated
```
- if unvisited and modified by callback, some() will pick up new value (tested in console)
```javascript
var testArray = [1, 2, 3];
some(testArray, function(number, index) {
  if (index === 0 ) {
    testArray[2] = 100
  }
  return number < 100;
})
// returns false
```
- callback is not invoked for deleted values
```javascript
var array = [1, 2, 3];
var callbackCounter = 0;
some(array, function(number, index) {
  if (index === 0 ) {
    delete array[3];
  }
  callbackCounter++;
  return number > 0;
})
// returns true
```
# Version 3
## Prototype Implementation
```javascript
function some(array, callback) {
  var isArrayTrue = false;
  for (var i = 0; i < array.length; i++) {
    isArrayTrue = callback(array[i]);
    if (isArrayTrue) { return isArrayTrue }; 
  }
  return isArrayTrue;
}
```
## My function declaration
some(array, callback, optionalThis)

## Callback parameters
element
currentIndex
originalArray

## Return 
Returns true if at least one element passes. Else, it returns false

## Requirements
- if callbacks condition passes at least 1 element, it should return true
- if callbacks condition does not pass for at least 1 element, it should return false
- if the array has a hole, it should not invoke the callback
- if elements are appended to array during run, it should ignore those elements
- if element is modified by callback and unvisited, it should use the modified value
- if element is deleted during run, it should not run the callback