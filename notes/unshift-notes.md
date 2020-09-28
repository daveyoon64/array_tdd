# Array.prototype.unshift Step 1
unshift() adds 1 or more elements to the beginning of array
returns new length of array

## example
```javascript
const array1 = [1, 2, 3];

console.log(array1.unshift(4, 5));
// expected output: 5

console.log(array1);
// expected output: Array [4, 5, 1, 2, 3]
```
## Syntax
arr.unshift(element1[, ...[, elementN]])

## Parameters
elementN: elements were adding to the front of arr

## Returns
length of new array

## Description
- inserts values at the beginning of array-like obj
- it's generic, so call can use it on array-like obj
- it's generic, so apply can use it on array-like obj 
- if it doesn't have a length property, it shouldn't work
- if passing in multiple elements, it's inserted as a chunk and in order they were passed in
```javascript
arr.unshift(1, 2, 3)
// vs 
arr.unshift(1)
arr.unshift(2)
arr.unshift(3)
```
but this should provides same results 
```javascript
arr.unshift(3)
arr.unshift(2)
arr.unshift(1)
```
- if you pass in an arraylike, it'll add the array as an element
- you won't have to recursively check for arrays in arrays

## Notes
- Pretty similar to shift(), except we're inserting at the first element
- but reindexing the array seems to be the difficult thing to do...
```javascript
[1, 2, 3, 4]
[1, 2, 3, 4, undefined]
          ^       ^
          |       |
          |       indexPtr
          |      
          searchPtr
```
- searchPtr is going right to left, looking for the beginning of array
- once is gets to the end, array[0] = undefined
- i have to shift over all elements over 1, so 0th is undefined
- pseudo
1. set searchPtr to last truthy element in array, set indexPtr to LAST position of array
2. array[indexPtr--] = index[searchPtr--] // setindexPtr value to searchPtr value, then move left
3. if searchPtr is 0, stop, and set searchPtr value to undefined
```javascript
[1, 2, 3, 4, undefined, undefined]
          ^       ^
          |       |
          |       indexPtr
          |      
          searchPtr
```
- so I ran into a problem with first approach.. if there's more than 1 elementN, then searchPtr
will overwrite the beginning of the array I want to be undefined
- the solution, I think is to update the arraylike in real-time and ensure undefined are promptly assigned
```javascript
searchPtr != 0 -> arraylike[searchPtr] //???
```

# Step 2 
## Prototype Implementation
```javascript
function unshift(arraylike, ...elementN) {
  var elementsToAdd = elementN.length;
  var startPosition = arraylike.length;

  // adds elementN.length elements to the arraylike
  for (var i = startPosition; i < startPosition + elementsToAdd; i++) {
    arraylike[i] = undefined;
  }

  arraylike.length = i;

  // shifts all values in arraylike to the right
  var searchPtr = startPosition - 1;
  var indexPtr = arraylike.length - 1;

  while (searchPtr >= 0) {
    arraylike[indexPtr--] = arraylike[searchPtr--];
  }

  // replaces the values in arraylike, starting from index 0
  for (var i = 0; i < elementN.length; i++) {
    arraylike[i] = elementN[i];
  }
  return arraylike.length
}
```
## Definition
unshift() inserts elementN args into the beginning of the arraylike in order

# Syntax
unshift(arraylike, elementN)

# Parameters
arraylike
elementN

# Returns
the new length of the expanded arraylike

## Requirements
unshift should insert elementN element at the 0th position of arraylike and shift the rest of the elements right
```javascript
unshift([1, 2, 3], 9)
// returns 4
// should be [9, 1, 2, 3]
```
if array is empty and elementN is empty, unshift should return 0
```javascript
var testArray = []
unshift(testArray)
// returns 0
// testArray should be an empty array
```
unshift should update arraylike.index.length
```javascript
var testArray = [1, 2, 3]
unshift(testArray, 4)
// returns 4
```
shift can be called on arraylike objects
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon', length: 4};
var testValue = unshift(myFish, 'clownbaby');
// testValue is 5
// clownbaby should be in position 0
```
shift is generic so it should work with call()
```javascript
var testArray = [1, 2, 3, 4]
unshift.call(null, testArray, 177)
// returns 5
// array should be [177, 1, 2, 3, 4]
```
shift is generic so it should work with apply()
```javascript
var testArray = [1, 2, 3, 4]
unshift.apply(null, [testArray, 999])
// returns 5
// array should be [999, 1, 2, 3, 4]
```
shift will not work reliably on objects without a length property
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon'};
var testValue = unshift(myFish, 'rick perry');
// testValue is undefined.
// myFish is unaltered
```
shift does not work with strings
```javascript
var isTypeError = false;
try {
  shift('dave', 'y')
} catch(e) {
  isTypeError = (e instanceof TypeError);
}
eq(isTypeError, true);
```
shift does not work with the arguments object
```javascript
var testArgs;
var isTypeError = false;
function argsTest(a, b, c) {
  testArgs = arguments;
}
argsTest(1, 2, 3);
try {
  shift(testArgs, 4) 
} catch (e) {
  isTypeError = (e instanceof TypeError)
}
eq(isTypeError, true);
```
# Step 3
## Prototype Implementation
```javascript
function unshift(arraylike, ...elementN) {
  var elementsToAdd = elementN.length;
  var startPosition = arraylike.length;

  // adds elementN.length elements to the arraylike
  for (var i = startPosition; i < startPosition + elementsToAdd; i++) {
    arraylike[i] = undefined;
  }

  arraylike.length = i;

  // shifts all values in arraylike to the right
  var searchPtr = startPosition - 1;
  var indexPtr = arraylike.length - 1;

  while (searchPtr >= 0) {
    arraylike[indexPtr--] = arraylike[searchPtr--];
  }

  // replaces the values in arraylike, starting from index 0
  if (indexPtr) {
    for (var i = 0; i < elementN.length; i++) {
      arraylike[i] = elementN[i];
    }
  }
  return arraylike.length
}
```
## Definition
unshift() inserts elementN args into the beginning of the arraylike in order

## Syntax
unshift(arraylike, elementN)

## Parameters
arraylike
elementN

## Returns
the new length of the expanded arraylike

## Requirements
- it should insert elements from elementN in order, starting at position 0 of the arraylike
- if array is empty and elementN is empty, it should return 0
- it should update arraylike.length
- it should be generic enough to use arraylike objects
- it should be generic enough to use with call()
- it should be generic enough to use with apply()
- it should not work on objects without a length property
- it should not work with strings
- it should not work with the arguments object