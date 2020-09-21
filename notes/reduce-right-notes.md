# Array.prototype.reduceRight() Step 1

reduceRight() method applies callback against accumulator and each array element
from right to left and reduce to a single value

## Example
```javascript
const array1 = [[0, 1], [2, 3], [4, 5]].reduceRight(
  (accumulator, currentValue) => accumulator.concat(currentValue)
);

console.log(array1);
// expected output: Array [4, 5, 2, 3, 0, 1]
```
## Syntax
arr.reduceRight(callback( accumulator, currentValue[,index[,array]])[, initialValue])

## Parameters
callback
initialValue (optional)

## Callback parameters
accumulator value returned from last invocation, or initialValue
currentValue
index
array

## Return
Value that results from right reduction

## Description
reduceRight() executes once for each element in array
- excludes holes in the array

There is an initialValue
```javascript
[0,1,2,3,4].reduceRight(function(accumulator, currentValue, index, array) { ... }, initialValue)
         ^
         |
         currentValue
accumulator = initialValue
```
There is no initialValue
```javascript
[0,1,2,3,4].reduceRight(function(accumulator, currentValue, index, array) { ... })
       ^ ^
       | |
       | initialValue set here
       |
       currentValue set here
```

Array is empty and no initialValue
[].reduceRight(function(accumulator, currentValue, index, array) { ... })
throws TypeError

Array has one element, regardless of position and no initialValue
[4].reduceRight(function(accumulator, currentValue, index, array) { ... })
return 4 without calling callback

Array is empty and there is an initialValue
[].reduceRight(function(accumulator, currentValue, index, array) { ... }, 42)
return 42 without calling callback

# Step 2
## Prototype Implementation
```javascript
function reduceRight(array, callback, initialValue) {
  var currentValueIndex = array.length - 1;
  var accumulator = initialValue;

  if (!initialValue) {
    currentValueIndex--;
    accumulator = array[array.length - 1]
  }

  for (currentValueIndex; currentValueIndex >= 0; currentValueIndex--) {
    accumulator = callback(accumulator, array[currentValueIndex]);
  }
  return accumulator;
}
```

## Function signature
reduceRight(array, callback, initialValue)

## Callback parameters
accumulator
currentValue
index
array

## Returns
a single value

## Requirements
if initialValue, the currentValue should be equal to the last element of array
```javascript
reduceRight([1, 2, 3], function() {}, 10)
// 3
```
if initialValue, the accumulator should be equal to initialValue
```javascript
reduceRight([1, 2, 3], function() {}, 10)
// 10
```
if initialValue, the callback shuould start at the last element
```javascript
reduceRight([1, 2], function() {}, 10)
// index is 1 
```
if no initialValue, the currentValue should be equal the second to last element of array
```javascript
reduceRight([1, 2, 3], function() {})
// 2
```
if no initialValue, the accumulator should be equal to the last element of array
```javascript
reduceRight([1, 2, 3], function() {})
// 3
```
if no initialValue, the callback should start at the second to last element of array
```javascript
reduceRight([1, 2, 3], function() {})
// index is 1
```

It returns the reduced value from right to left
```javascript
reduceRight([1, 2, 3], function(acc, cur) {
  return acc + cur;
}, 10)
// 16
```

if array is empty and no initialValue, throw a TypeError
```javascript
reduceRight([], function() {})
// TypeError
```
if array is empty, and there is an initialValue, return initialValue
```javascript
reduceRight([], function() {}, 10)
// 10 no callback
```
if array has 1 element and no initialValue, return element
```javascript
reduceRight([3], function() {})
// 3 no callback
```
it should exclude holes in the array
```javascript
reduceRight([,,,,1,undefined,2, 3], function() {}, 10)
// 16
// callback should only run 3 times?
```

# Step 3
## Prototype Implementation
```javascript
function reduceRight(array, callback, initialValue) {
  var currentValueIndex = array.length - 1;
  var accumulator = initialValue;

  if (!initialValue) {
    currentValueIndex--;
    accumulator = array[array.length - 1]
  }

  for (currentValueIndex; currentValueIndex >= 0; currentValueIndex--) {
    accumulator = callback(accumulator, array[currentValueIndex]);
  }
  return accumulator;
}
```

## Function signature
reduceRight(array, callback, initialValue)

## Callback parameters
accumulator
currentValue
index
array

## Returns
a single value

## Requirements
if initialValue, currentValue should be equal to last element of array
if initialValue, accumulator should be equal to initialValue
if intialValue, the callback should start at the last element

if no initialValue, the currentValue should be equal to the second to last array element
if no initialValue, the accumulator should be equal to the last element of array
if no initialValue, the callback should start at the second to last element of array

it should return the reduced value from right to left

if array is empty and has no initialValue, it should throw a TypeError
if array is empty and there is an initialValue, it should return initialValue
if array has 1 element and has no initialValue, it should return the element

it should exclude holes in the array