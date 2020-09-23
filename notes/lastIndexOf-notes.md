# Array.prototype.lastIndexOf

# Step 1
lastIndexOf() returns last index where element is found
returns index or -1 if not found

array is search backwards, starting at fromIndex

## Example
```javascript
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

console.log(animals.lastIndexOf('Dodo'));
// expected output: 3

console.log(animals.lastIndexOf('Tiger'));
// expected output: 1
```
## Syntax
arr.lastIndexOf(searchElement 9, fromIndex])

## Parameters
searchElement: what we're trying to locate in the array
fromIndex: where to start searching, but backwards
	- defaults to array.length - 1
	- if index >= array.length, whole array is searched
	- if fromIndex < 0, it's taken as offset from the end of the array
		- it is still searched from back to front
		- if calculated index < 0, return -1 & array is not searched

## Return value
last index of element in array, -1 is not found

## Description
searchElement is compared using strict equality (===)

# Step 2
## Prototype Implementation
```javascript
function lastIndexOf(array, searchElement) {
  var startIndex = array.length - 1;

  for (var i = startIndex; i >= 0; i--) {
    if (array[i] === searchElement) {
      return i;
    }
  }
  return - 1;
}
```

## Function signature
lastIndexOf(array, searchElement, fromIndex)

## Parameters
array: array to search, from right to left
searchElement: element (rightmost) we're trying to find
fromIndex: where to start, function still searches right to left

## Requirements
- returns the rightmost index if found
```javascript
lastIndexOf([1, 2, 1], 1)
// return 2
```
- if searchElement not found, return -1
```javascript
lastIndexOf([1, 2, 1], 3)
// return -1
```
- if no fromIndex, start at array.length -1
```javascript
lastIndexOf([7, 1, 1, 1], 7)
// return 0
```
- if fromIndex > array.length, search the entire array
```javascript
lastIndexOf([3, 2, 1, 1], 3, 20)
// return 0
```
- if fromIndex < 0, it'll take it as an offset from end of array
```javascript
[1, 2, 3, 4]  // we can get this position with array.length + fromIndex
          ^
          |
          |
          position 3, but also -1
lastIndexOf([1, 2, 1], 1, -2) // starts at index position 1
// return 0
```
- if fromIndex < 0, lastIndexOf() still searches from back to front
```javascript
lastIndexOf([1, 1, 1], 1, -1)
// return 2
// it was in docs, but I feel that all the test implicitly test the right to left feature
```
- if fromIndex < 0, and after the offset is calculated, if index is still negative, then return -1
```javascript
lastIndexOf([1, 2, 1], -6)
// return -1
```
- searchElement is compared using strict equality
```javascript
lastIndexOf(['1', 2, '1'], 1)
// return -1
```

# Step 3
## Prototype Implementation
```javascript
function lastIndexOf(array, searchElement) {
  var startIndex = array.length - 1;

  for (var i = startIndex; i >= 0; i--) {
    if (array[i] === searchElement) {
      return i;
    }
  }
  return - 1;
}
```

## Function signature
lastIndexOf(array, searchElement, fromIndex)

## Parameters
array
searchElement
fromIndex

## Requirements
- if searchElement found, it should return the rightmost instance
- if searchElement not found, it should return -1
- if no fromIndex, it should start at array.length - 1
- if fromIndex, it should start its search from that array position
- if fromIndex > array.length, it should search the entire array
- if fromIndex < 0, it should take it as an offset from array end (array.length + fromIndex)
- if fromIndex < 0, it should still search right to left
- if fromIndex < 0, and the offset is still negative, then return -1
- it should use strict equality
