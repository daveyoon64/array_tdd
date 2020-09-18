# findIndex() Step 1

## Summary
findIndex() returns index of the first element that satisfies the callback
otherwise it returns -1

## Example
```javascript
const array = [5, 12, 8, 130, 44];
const isLargeNumber = (element) => element > 13;
console.log(array.findIndex(isLargeNumber));
// output: 3
```

## Syntax
arr.findIndex(callback( element[, index[, array]] )[, thisArg])

## Callback Parameters
the value of element
current index of element
original array

## Return
returns index of first element in array that passes test, otherwise -1

## Description
- executes the callback once for each index in array until a truthy value
- if found, immediately returns index
- not found, return -1
- if array's length is 0, return -1
- like find() it'll run on arrays with holes (indexes with unassigned value)

## Edge Cases
elements processed by findIndex set BEFORE findIndex() invocation
- callback will not process elements appended to array after findIndex() begins
- if existing, unvisited element is modified by callback, that value will be used when findIndex() reaches it


# Step 2
## Prototype implementation
```javascript
var foundNumber;
function findIndex(array, callback) {
  for (var i = 0; i < array.length; i++) {
    foundNumber = callback(array[i], i, array);
    if (foundNumber) {
      return i; // just return the index
    }
  }
  return -1;
}
```

## Function definition
findIndex(array, callback, optionalThis)

## Callback parameters
currentElement 
currentIndex
originalArray

## Return
the index, else -1

## Case 1: an element in the value passes the callback's condition, return the index
```javascript
findIndex([1, 2, 3], function(number) {
  return number > 2;
})
```

## Case 2: no element is found, so -1 is returned
```javascript
findIndex([1,2,3], function(number) {
  return number > 5;	
})
```

## Case 3: if the array's length is 0, return -1
```javascript
findIndex([], function(number) {
  return number > 1;
})
```

## Edge cases:
### findIndex() should return an index, despite holes or indexes with unassigned values
```javascript
findIndex([,,,1,,,,4], function(number) {
  return number > 1;
})
```

### After starting, findIndex() should not process elements appended to array
If element exists, is unvisited, and is modified by the callback, the new value should be used when findIndex() gets there

# Step 3
## Prototype implementation
```javascript
var foundNumber;
function findIndex(array, callback) {
  for (var i = 0; i < array.length; i++) {
    foundNumber = callback(array[i], i, array);
    if (foundNumber) {
      return i; // just return the index
    }
  }
  return -1;
}
```

## Function Signature
findIndex(array, callback, optionalThis)

## Callback parameters
- currentElement 
- currentIndex
- originalArray

## Return
- The index of first found element, else -1

## Requirements
- if the first element found passes a callback's test, it should return the element's index
- if no element passes a callback's test, it should return -1
- if the array's length is 0, it should return -1
- if the array has unassigned indexes, it should iterate through all indexes anyway
- after starting, it should element appended by the callback 
- if the element exists, is unvisited, and modified by the callback, it should use the new value 

