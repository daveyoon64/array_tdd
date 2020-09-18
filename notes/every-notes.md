# Array.prototype.every() Part 1

## Definition
every() method tests all array elements with callback
if it passes, it return a boolean

## Example
```javascript
const isBelowThreshold = (currentValue) => currentValue < 40;
const array1 = [1, 30, 39, 29, 10, 13];
console.log(array1.every(isBelowThreshold));
// returns true
```

## Syntax
arr.every(callback(element[, index][, array]])[, thisArg]])

## Parameters
callback(element being processed, index of current element, original array)

## Return
true if the callback returns something truthy
otherwise false

## Description
- calls on every element UNTIL it finds an element that returns falsy, immediately returns false
- if all elements pass true, return true
- calling every() on an empty array returns true no matter what
- every() is called only for indexes with assigned values (deleted, holes, etc are ignored)
- every() does not mutate the array
- every() will not run on elements appended to array after the every() call
- if existing elements changed... wtf??
  - values passed to callbackwill be the value at time when every() visits
  - probably means if it's unvisited and modified, the value should be the modified one (will test in stage 2)
- deleted elements are not visited

# Version 2
## Prototype Implementation
```javascript
function every(array, callback) {
  var isArrayTrue = true; 
  for (var i = 0; i < array.length; i++) {
    isArrayTrue = callback();
    if (!isArrayTrue) {
      return isArrayTrue;
    }
  }
  return isArrayTrue;
}
```

## My function declaration
every(array, callback, optionalThis)

## Callback parameters
element
currentIndex
originalArray

## Returns 
Returns true if all elements pass, else false

## Description
- call on every element in array. if falsy, immediately returns false
```javascript
every([1, 2, 3], function(number) {
  return number > 5;
})
// returns false
```
- if all elements are true, return true
```javascript
every([1, 2, 3], function(number) {
  return number > 0;
})
// returns true
```
- an empty array returns true
```javascript
every([], function(number) {
  return number > 5;
})
// returns true
```
- indexes with no assigned values are ignored
```javascript
every([,,1, 2, undefined, 3], function(number) {
  return number > 0;
})
// returns true
// undefined > 0, '' > 0, are both false, so if we return true it'll be good
```
- will not run on elements appended to array during run
```javascript
var testArray = [1, 2, 3];
every(testArray, function(number) {
  testArray.push(20);
  return number < 10;
})
// returns true, since the 20 is never evaluated
```
- if unvisited and modified by callback, every() will pick up new value (tested in console)
```javascript
var testArray = [1, 2, 3];
every(testArray, function(number, index) {
  if (index === 0 ) {
    testArray[2] = 100
  }
  return number < 99;
})
// returns false
```
- deleted elements will not be visited
```javascript
var testArray = [1, 2, 3];
every(testArray, function(number, index) {
  if (index === 0 ) {
    delete testArray[2];
  }
  return number < 3;
})
// returns true, since position 2 no longer exists
```
# Version 3
## Prototype Implementation
```javascript
function every(array, callback) {
  var isArrayTrue = true; 
  for (var i = 0; i < array.length; i++) {
    isArrayTrue = callback();
    if (!isArrayTrue) {
      return isArrayTrue;
    }
  }
  return isArrayTrue;
}
```
## My function declaration
every(array, callback, optionalThis)

## Callback parameters
element
currentIndex
originalArray

## Return 
Returns true if all elements pass, else false

## Requirements
- if callbacks condition passes for all elements, it should return true
- if callbacks condition fails for any element, it should return false
- if the array is empty, it should return true
- if array has undefined indexes (holes), it should not run the callback
- if elements are appended to array during run, it should ignore those elements
- if element is modified by callback and unvisited, it should use the modified value
- if element is deleted during run, it should not run the callback