# Array.prototype.pop Step 1

pop() removes the last element from array and returns it
it changes length of array

## example
```javascript
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato'];

console.log(plants.pop());
// expected output: "tomato"

console.log(plants);
// expected output: Array ["broccoli", "cauliflower", "cabbage", "kale"]
```
## Syntax
arr.pop()

## Return
removed element from array
undefined if array empty

## Description
- pop() is pretty generic and can be called or applied to arraylike objects
- will not work with objects with no length property 
- it should then be generic enough to use will call() and apply()

## Notes
- to use apply() or call() on array-like objects... we gotta use function expressions instead of declarations
var pop = function(array) {
   ...
}
- two ways to do this, just use splice or we'll have to reindex the array inplace and update the length
https://stackoverflow.com/questions/28582543/reindexing-array-of-objects-in-javascript-no-splice



# Step 2
## Prototype Implementation
```javascript
// won't work for arraylikes
var pop = function(array) {
  var lastValue = array[array.length-1];
  array.splice(array.length - 1, 1);
  return lastValue;
}

// splice doesn't like this splice
function pop(arraylike) {
  var newArray = Array.from(arraylike);

  var lastValue = newArray[newArray.length-1];
  //newArray.splice(newArray.length - 1, 1);
  arraylike.splice(arraylike.length - 1, 1); // since pop modifies the original array
  return lastValue;
}

// this will work. 
// but I have to get creative with the mutation of the
// source array
function pop(arraylike) {
  var newArray = Array.from(arraylike);
  var indexToDelete = newArray.length-1;

  // this still works great
  var lastValue = newArray[indexToDelete];

  // the tricky part will be updating the arraylike in-place
  // and updating length

  // delete works, but the problem is that it leaves a hole
  // in array that says undefined
  // example: [1, 2, 3, undefined], if I popped arraylike[4]
  delete arraylike[indexToDelete];

  for (var i = 0; i < arraylike.length; i++) {
    filterInPlace(arraylike, function(value) {
      if (value) { return true; }
    });
  }

  return lastValue;
}
function filterInPlace(arraylike, callback) {
  let i = 0, j = 0;

  while (i < arraylike.length) {
    const val = arraylike[i];
    if (callback(val, i, arraylike)) arraylike[j++] = val;
    i++;
  }

  a.length = j;
  return a;
}
```
## Syntax
pop(array)

## Parameters
array: the array thats poppin off

## Return
last element of the array
else returns undefined

## Requirements
- pop should return the last element of the arraylike
```javascript
pop([1, 2, 3])
// returns 3
```
- if array is empty, return undefined
```javascript
var testValue = pop([]])
// return undefined
```
- pop should remove the last element of the arraylike
```javascript
var testArray = [1, 2, 3]
pop(testArray)
// testArray = [1, 2]
```
- pop should update arraylike.index.length
```javascript
var testArray = [1, 2, 3]
pop(testArray)
// testArray.length = 2
```
- pop can be called on arraylike objects
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon', length: 4};
var testValue = pop(myFish);
// testValue is sturgeon
// myFish.length is 3
```
- pop is generic so it should work with call()
```javascript
var testArray = [1, 2, 3, 4]
pop.call(null, testArray)
// returns 4
```
- pop is generic so it should work with apply()
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon', length: 4};
pop.apply(null, [myFish]])
// returns 'sturgeon', size is 3
```
- pop will not work reliably on objects without a length property
```javascript
var myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon'};
var testValue = pop(myFish);
// testValue is undefined.
// myFish is unaltered
```
- pop does not work with strings
```javascript
var isTypeError = false;
try {
  push('dave', 'y')
} catch(e) {
  isTypeError = (e instanceof TypeError);
}
eq(isTypeError, true);
```
- pop does not work with the arguments object
```javascript
var testArgs;
var isTypeError = false;
function argsTest(a, b, c) {
  testArgs = arguments;
}
argsTest(1, 2, 3);
try {
  push(testArgs, 4)	
} catch (e) {
  isTypeError = (e instanceof TypeError)
}
eq(isTypeError, true);
```

# Step 3
## Prototype Implementation
```javascript
function pop(arraylike) {
  var newArray = Array.from(arraylike);
  var indexToDelete = newArray.length-1;
  var lastValue = newArray[indexToDelete];

  delete arraylike[indexToDelete];

  debugger;
  filterInPlace(arraylike, function(value) {
    if (value) { 
      return true;
    }
  });
  return lastValue;
}

function filterInPlace(arraylike, callback) {
  let i = 0, j = 0;

  while (i < arraylike.length) {
    const val = arraylike[i];
    if (callback(val, i, arraylike)) { 
      arraylike[j++] = val;
    }
    i++;
  }
  // you have to update length, or the empty value won't go away!!
  arraylike.length = j;
}
```

## Syntax
pop(arraylike)

## Parameters
Any arraylike object with length. Cannot be a string or arguments object
 
# Returns
last element of the arraylike
if not found returns undefined

## Requirements
- it should return the last element of the arraylike
- if array is empty, it should return undefined
- it should remove the last element of the arraylike
- it should update arraylike.index.length
- it should be generic enough to use arraylike objects
- it should be generic enough to use with call()
- it should be generic enough to use with apply()
- it should not work on objects without a length property
- it should not work with strings
- it should not work with the arguments object

