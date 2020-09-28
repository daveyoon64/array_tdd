# Array.prototype.fill() Step 1
fill() changes all array elements to a static value from 0 to end of array
it returns modificed array

## Example
```javascript
const array1 = [1, 2, 3, 4];

// fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4));
// expected output: [1, 2, 0, 0]

// fill with 5 from position 1
console.log(array1.fill(5, 1));
// expected output: [1, 5, 5, 5]

console.log(array1.fill(6));
// expected output: [6, 6, 6, 6]
```
## Syntax
arr.fill(value[, start[, end]])

## Parameters
value: what to fill the array with
start: position to start
end: when to end (exclusive)

## Return
modified array, filled with value

## Description
if given array and value, replace array elements with value
if given array, value and start, replace array elements starting at start
if given array, value, start, and end replace array elements from start o end
if start < 0, calculate start position with array.length + start
if end < 0, calculate start position with array.length + end
fill is generic, so it doesn't need this value (call and apply should work)
fill is a mutator and will change the array
if value is an object, each slot in the array will reference that object
```javascript
let arr = Array(3).fill({}) // [{}, {}, {}]
arr[0].hi = "hi"            // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```
if start or end are undefined, replace array element with value
// still sort of works with undefined
if start or end are NaN or null, do nothing
```javascript
test = fill([1, 2, 3], 7, NaN, NaN);
// [1, 2, 3]
```
if start and end are > array.length, is should do nothing
You can send in a built-in array and fill it
```javascript
Array(2).fill(4)
```
it should be generic enough to use apply and call
```javascript
// [].fill.call({ length: 3 }, 4)   // {0: 4, 1: 4, 2: 4, length: 3}
var testObj = fill.call(null, {length: 3}, 4)
{0: 4, 1: 4, 2: 4, length: 3}
```
# Step 2 
## Prototype Implementation
```javascript
function fill(array, value, start, end) {
  var startPos;
  var endPos;

  if (Number.isInteger(start)) { 
    startPos = start;
    if (start < 0) startPos = array.length + start;
  } else if (start === undefined) {
    startPos = 0;
  }
  if (Number.isInteger(end)) { 
    endPos = end;
    if (end < 0) endPos = array.length + end;
    if (endPos > array.length) endPos = array.length;
  } else if (end === undefined) {
    endPos = array.length;
  }

  for (var i = startPos; i < endPos; i++) {
    array[i] = value;
  }
  return array;
}
```

## Description
fill() changes elements in array to value provided
from start (position) to end (position) if specified
0 to array.length if no specifie

## Syntax
fill(value, start, end)

## Parameters
value
start
end

## Returns
Mutated array

## Requirements
if given array and value, replace array elements with value
```javascript
fill([1, 2, 3], 7)
// [7, 7, 7]
```
if given array, value and start, replace array elements starting at start
```javascript
fill([1, 2, 3], 8 , 1)
// [1, 8, 8]
```
if given array, value, start, and end replace array elements from start to end (exclusive)
```javascript
fill([1, 2, 3], 9, 1, 2)
// [1, 9 , 2]]
```
if start < 0, calculate start position with array.length + start
```javascript
fill([1, 2, 3], 8, -3)
// [8, 8, 8
```
if end < 0, calculate start position with array.length + end
```javascript
fill([1, 2, 3], 7, 0, -1)
// [7, 7, 3]
```
fill is a mutator and will change the array
```javascript
var testArray = [1, 2, 3];
var returnArray = fill(testArray, 7);
// testArray === returnArray
```
if value is an object, each slot in the array will reference that object
```javascript
let arr = Array(3).fill({}) // [{}, {}, {}]
arr[0].hi = "wassup"            
// [{ hi: "wassup" }, { hi: "wassup" }, { hi: "wassup" }]
```
if start is undefined, replace start with 0
```javascript
fill([1, 2, 3], 777, undefined)
// [777, 777, 777]
```
if end is undefined, replace end with array.length
```javascript
fill([1, 2, 3], 333, 2, undefined)
// [1, 2, 333]
```
if start or end are NaN or null, do nothing
```javascript
test = fill([1, 2, 3], 7, NaN, NaN);
// [1, 2, 3]
```
if start  > array.length, it should do nothing
```javascript
fill([1, 2, 3], 5)
// [1, 2, 3]
```
if end > array.length, it should not affect array beyond array.length
```javascript
fill([3, 4, 5], 6, 0, 7)
// [6, 6, 6]
```
it should be generic enough to use apply and call
```javascript
var testObj = fill.call(null, {length: 3}, 4)
// {0: 4, 1: 4, 2: 4, length: 3}
```

# Step 3
```javascript
function fill(array, value, start, end) {
  var startPos;
  var endPos;

  if (Number.isInteger(start)) { 
    startPos = start;
    if (start < 0) startPos = array.length + start;
  } else if (start === undefined) {
    startPos = 0;
  }
  if (Number.isInteger(end)) { 
    endPos = end;
    if (end < 0) endPos = array.length + end;
    if (endPos > array.length) endPos = array.length;
  } else if (end === undefined) {
    endPos = array.length;
  }

  for (var i = startPos; i < endPos; i++) {
    array[i] = value;
  }
  return array;
}
```
## Description
fill() changes elements in array to value provided
from start (position) to end (position) if specified
0 to array.length if no specifie

## Syntax
fill(array, value, start, end)

## Parameters
value
start
end

## Returns
Mutated array

## Requirements
if given array and value, replace array elements with value
if given array, value and start, replace array elements starting at start
if given array, value, start, and end replace array elements from start to end (exclusive)

if given array and no value, it should replace elements with undefined
if array is empty, it should return []

if start < 0, calculate start position with array.length + start
if end < 0, calculate start position with array.length + end

It should mutate the original array
if value is an object, each slot in the array should reference that object
if start is undefined, it should replace start with 0
if end is undefined, it should replace end with array.length
if start or end are NaN or null, it should do nothing
if start > array.length, it should do nothing
if end > array.length, it should not affect array beyond array.length

it should be generic enough to use apply
it should be generic enough to use call