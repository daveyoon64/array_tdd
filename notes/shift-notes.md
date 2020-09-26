# Array.prototype.shift

shift() removes the first element from array and returns it
it changes the length of the array

## Example
```javascript
const array1 = [1, 2, 3];

const firstElement = array1.shift();

console.log(array1);
// expected output: Array [2, 3]

console.log(firstElement);
// expected output: 1
```
## Syntax
arr.shift()

## Return
the removed element, else return undefined

## Description
- removes 0th index and shifts the values at consecutive indexes down, then returns the removed value
- if length property is 0, return undefined
- it's generic, so call() should work with arraylike objects
- it's generic, so apply() should work with arraylike objects
- if it doesn't have length, it won't work with shift bro


# Step 2
## Prototype Implementation
```javascript
function shift(arraylike) {
  var newArray = Array.from(arraylike);
  var indexToDelete = 0;

  var firstValue = newArray[indexToDelete];

  delete arraylike[indexToDelete];

  for (var i = 0; i < arraylike.length; i++) {
    filterInPlace(arraylike, function(value) {
      if (value) { return true; }
    });
  }

  return firstValue;
}
function filterInPlace(arraylike, callback) {
  let i = 0, j = 0;

  while (i < arraylike.length) {
    const val = arraylike[i];
    if (callback(val, i, arraylike)) arraylike[j++] = val;
    i++;
  }

  arraylike.length = j;
}
```

## Syntax
shift(arraylike)

## Parameters
arraylike object

## Returns
first element of arraylike, undefined otherwise

## Requirements
shift should return the last element of the arraylike
```javascript
shift([1, 2, 3])
// returns 1
```

if array is empty, return undefined
```javascript
var testValue = shift([]])
// return undefined
```
shift should remove the first element of the arraylike
```javascript
var testArray = [1, 2, 3]
shift(testArray)
// testArray = [2, 3]
```
shift should update arraylike.index.length
```javascript
var testArray = [1, 2, 3]
shift(testArray)
// testArray.length = 2
```
shift can be called on arraylike objects
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon', length: 4};
var testValue = shift(myFish);
// testValue is angel
// myFish.length is 3
```
shift is generic so it should work with call()
```javascript
var testArray = [1, 2, 3, 4]
shift.call(null, testArray)
// returns 1
```
shift is generic so it should work with apply()
```javascript
var myFish = {0:'crab', 1:'clown', 2:'mandarin', 3:'sturgeon', length: 4};
shift.apply(null, [myFish]])
// returns 'crab', size is 3
```
shift will not work reliably on objects without a length property
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon'};
var testValue = shift(myFish);
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
function shift(arraylike) {
  var newArray = Array.from(arraylike);
  var indexToDelete = 0;
  var firstValue = newArray[indexToDelete];

  delete arraylike[indexToDelete];

  for (var i = 0; i < arraylike.length; i++) {
    filterInPlace(arraylike, function(value) {
      if (value) { return true; }
    });
  }
  return firstValue;
}
function filterInPlace(arraylike, callback) {
  let i = 0, j = 0;

  while (i < arraylike.length) {
    const val = arraylike[i];
    if (callback(val, i, arraylike)) arraylike[j++] = val;
    i++;
  }
  arraylike.length = j;
}
```
## Syntax
shift(arraylike)

## Parameters
arraylike object

## Returns
first element of arraylike, undefined otherwise

Requirements
- it should return the first element of the arraylike
- if array is empty, it should return undefined
- it should remove the first element of the arraylike
- it should update arraylike.index.length
- it should be generic enough to use arraylike objects
- it should be generic enough to use with call()
- it should be generic enough to use with apply()
- it should not work on objects without a length property
- it should not work with strings
- it should not work with the arguments object

