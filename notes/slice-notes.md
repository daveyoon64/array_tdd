# Array.prototype.slice() Step 1
slice() returns a shallow copy of a portion of an array into new array object
you can specify the start and end of where to slice()

original array is not modified

## Example
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
```javascript
console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]
// end position is not inclusive

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]
// if index out of range, it just goes to the end
```

## Syntax
arr.slice([ , start[, end]])

## Parameters
start
	- undefined, then starts at 0
	- can be negative (using array.length + start offset)
		- ex. -2 will extract the last 2 elements
    - start > index range, return empty array
end
	- slices up to BUT NOT INCLUDING end
	- can be negative (using array.length + end offset)
		- slice(2, -1) extracts the 3rd element to the 2nd to last element
	- if no end, slice goes to end of arry
	- if end > length of sequence, slice goes to end of array

## Returns
a new array w/ extracted elements

## Description
- slice does not alter original array
- returns shallow copy of elements from original array
- object references are copied in, both the original and new array will refer to same object
- if referenced object changes, it'll be visible in new and original arrays
- for primitives, the values are copied and are unique to each array
- newly added elements to either array will not affect the other

# Step 2
## Prototype Implementation
```javascript
function slice(array, start, end) {
  var positionStart = 0;
  var positionEnd = array.length;
  var newArray = [];

  if (start) {
    positionStart = start;
    if (start < 0) {
      positionStart = array.length + start;
    }
  }
  if (end) {
    positionEnd = end;
    if (end < 0) {
      positionEnd = array.length + end;
    }
  }

  for (var i = positionStart; i < positionEnd; i++) {
    newArray.push(array[i]);
  }

  return newArray;
}
```
## Function Signature
slice(array, start, end)

## Parameters
start
end

## Returns
array with sliced values

## Requirements
- if we have a start, slice returns a new array from start to the end of the array
```javascript
slice([1, 2, 3], 1)
// [2, 3]
```
- if we have a start and end, slice returns a new array from array[start] to array[end] (exclusive)
```javascript
slice([1, 2, 3], 1, 2)
// [2]
```
- if no start, slice will return a shallow copy of array
- if start is negative, use an offset (array.length + start) to determine where to start the slice
```javascript
slice([1, 2, 3], -2,)
// [2, 3]
```
- if start > array.length - 1, return empty array
```javascript
slice([1, 2, 3], 3)
// []
```
- if end is negative, use an offset (array.length + start) to determine where to end the slice
```javascript
slice([1, 2, 3], 0, -2)
// [1]
```
- if no end, slice will go to the end of the array
- if end > length of sequence, slice will go to the end of the array
```javascript
slice([1, 2, 3], 1, 100)
// [2, 3]
```
- slice does not alter original array
- slice returns a shallow copy, so the new and original arrays will share object references
```javascript
var array = [1, 2, [3]]
var testSlice = slice(array)
testSlice[2][0] = 1776
// array[2][0] will be 1776
```
- primitives copied to the array are unique to the respective arrays
- newly added elements to either array will not affect the other
```javascript
var array = [1, 2, 3]
var testSlice = slice(array)
testSlice.push(12);
// array[3] will be undefined
```
# Step 3
## Prototype Implementation
```javascript
function slice(array, start, end) {
  var positionStart = 0;
  var positionEnd = array.length;
  var newArray = [];

  if (start) {
    positionStart = start;
    if (start < 0) {
      positionStart = array.length + start;
    }
  }
  if (end) {
    positionEnd = end;
    if (end < 0) {
      positionEnd = array.length + end;
    }
  }

  for (var i = positionStart; i < positionEnd; i++) {
    newArray.push(array[i]);
  }
  return newArray;
}
```

## Function Signature
slice(array, start, end)

## Parameters
start
end

## Returns
array with sliced values

## Requirements
- if start exists, it should return a copy of array from start index to array end
- if no start, it should return a copy of the array
- if start and end exist, it should return a copy of the array from start to end (exclusive)

- if start < 0, it should use the start offset (array.length+start) to return a copy of array from start to array end
- if start > array.length - 1, return empty array
- if end < 0, it should use the end offset (array.length+end) to return a copy of array from start to end
- if no end, it should return a copy of array from start to array end
- if end > array.length - 1, it should return a copy of array from start to array end

- it should return a shallow copy with shared object references
- it should return a shallow copy with primitives unique to each array
- added elements to either array should affect the other