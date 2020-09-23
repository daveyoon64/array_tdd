# Array.prototype.indexOf()

## Step 1 Reading and Notes
indexOf() returns first index at which given element can be found in array

## Return
index of element, else -1 if not present

```javascript
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];

console.log(beasts.indexOf('bison'));
// expected output: 1

// start from index 2
console.log(beasts.indexOf('bison', 2));
// expected output: 4

console.log(beasts.indexOf('giraffe'));
// expected output: -1
```

## Syntax
arr.indexOf(searchElement[, fromIndex])

## Parameters
searchElement: element to locate
fromIndex: where to start the search at
	- index >= array's length, return -1 (array not searched)
	- if 0, entire array is searched
	- if index < 0, it's an offset from end of array, but still searches from front to back

## Description
uses strict equality (===) to compare elements in array

# Step 2
## Prototype implementation
```javascript
function indexOf(array, elementToFind, startPosition) {
  var startIndex = 0;
  if (startPosition) {
    startIndex = startPosition;

    // read about JS arrays and apparently there's no such thing as out of bounds error in JS
    // negative values return undefined. just catch that?
    if (startPosition < 0) {
      startIndex = startPosition + array.length;
    }
  }
  
  for (var i = startIndex; i < array.length; i++) {
    if (array[i] === elementToFind) {
      return i;
    }
  }
  return -1;
}
```

## Function Signature
function indexOf(array, elementToFind, startPosition)

## Parameteres
array
elementToFind
startPosition

## Return
value of index, else -1 if not found

## Requirements
if the elementToFind is found in array, return its index
```javascript
indexOf([1, 2, 3], 3);
// returns 2
```
if the elementToFind is not found in array, return -1
```javascript
indexOf([1, 2, 3], 4);
// returns -1
```
if startPosition, array starts at startPosition and if elementToFind is found, return its index
```javascript
indexOf([1, 2, 3], 3, 2);
// returns 2
```
if startPosition, array starts at startPosition and if elementToFind is not found, return -1
```javascript
indexOf([1, 2, 3], 2, 2);
// returns -1
```
if startPosition is negative, add array.length to determine it's startPosition, and if elementToFind is found, return its index
```javascript
indexOf([1, 2, 3], 3, -1);
// returns 2
```
if startPosition is greater than array's length, don't search array and return -1
```javascript
indexOf([1, 2, 3], 3, 100);
// returns -1
```
if startPosition is 0, search the entire array
```javascript
indexOf([1, 2, 3], 3, 0);
// returns 2
```

# Step 3
## Prototype implementation
```javascript
function indexOf(array, elementToFind, startPosition) {
  var startIndex = 0;
  if (startPosition) {
    startIndex = startPosition;

    // read about JS arrays and apparently there's no such thing as out of bounds error in JS
    // negative values return undefined. just catch that?
    if (startPosition < 0) {
      startIndex = startPosition + array.length;
    }
    // after some experiementation, this isn't needed. If this gets a negative value, JS will
    // just start at that negative index. Basically, it just goes WIDE left of the array.
    // [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5], 
    // thinking about this, I have to test it with multiple values
  }
  
  for (var i = startIndex; i < array.length; i++) {
    if (array[i] === elementToFind) {
      return i;
    }
  }
  return -1;
}
```

## Function Signature
function indexOf(array, elementToFind, startPosition)

## Parameters
array
elementToFind
startPosition

## Return
value of index, else -1 if not found

## Requirements
- if the elementToFind is found in array, it should return its index
- if the elementToFind is not found in Array, return -1
- if startPosition, array starts at startPosition and if elementToFind is found, it should return its index
- if startPosition, array starts at startPosition and if elementToFind is not found, it should return -1
- if startPosition is negative, add array.length to determine its startPosition, and if elementToFind is found, it should return its index
- if startPosition is greater than array's length, it should not search array and return -1
- if startPosition is 0, it should search the entire array