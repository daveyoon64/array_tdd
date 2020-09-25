# Array.prototype.push() Step 1

push() adds one or more elements to the end of array

returns new length of array

## Example
```javascript
const animals = ['pigs', 'goats', 'sheep'];

const count = animals.push('cows');
console.log(count);
// expected output: 4
console.log(animals);
// expected output: Array ["pigs", "goats", "sheep", "cows"]

animals.push('chickens', 'cats', 'dogs');
console.log(animals);
// expected output: Array ["pigs", "goats", "sheep", "cows", "chickens", "cats", "dogs"]
```
## Syntax
arr.push([element[, ...[, elementN]]])

## Parameters
- element: what to push to array
- elementN: the unknown number of elements to add as end of array

## Returns
- new length property of object upon which the method was called

## Description
- appends values to an array
- push is generic
	- can be used with call() or apply() on array-like objects
	- looked into this, I'm not really sure how to do this... like at all
	- I supposed if I were to make this a method?
- push relies on length property to determine where to start inserting given values
- if length can't be converted to number, index 0 is used
- if length doesn't exist, create the property!

## Can't use push()
- strings should be unaffeted, because they're immutable
- arguments object

## My Experiments
- pushing on array just adds the array to the arr.push
```javascript
sports.push [["soccer", "baseball", "football"], ["swimming", "running", "diving"]]
// ["soccer", "baseball", "football", "swimming", "running", "diving", "jumping", Array(3), Array(3)]
```
## Outside my paygrade... appending array to another?
- you can push on an arraylike of elements... but to add on an arraylike, you have to apply() (as array) or call() (as a list)
- I can append another array, but I'm not sure how to make it work with apply() or call()

# Step 2 
## Prototype Implememtation
```javascript
function push(arraylike, ...elementN) {
  var endOfArraylike = arraylike.length;
  var indexesToAdd = endOfArraylike + elementN.length;
  var elementNPosition = 0;

  for (var i = endOfArraylike; i < indexesToAdd; i++) {
    arraylike[i] = elementN[elementNPosition];
    elementNPosition++;
  }
  return i;
}
```
## Syntax
push(arraylike, ...elementN)

## Parameters
arraylike
elementN

## Returns
length of new arraylike object

## Description
- this modifies the arraylike and adds new indexes and corresponding N elements
- push should work with any arraylike elements
- push relies on the length property to determine where to start inserting

- if length can't be converted to number, use index 0
```javascript
var daveOBJ = {name: 'Dave', length: 'hbberdy'};
push(daveOBJ, 7, 8);
// returns {0: 7, 1: 8, name: "Dave", length: 0}
```
- if length doesn't exist, create the property
```javascript
var daveOBJ = {name: 'Dave'};
push(daveOBJ);
// returns {name: "Dave", length: 0}
```
- does not work on strings object
```javascript
try {
  push('dave', 'y');
} catch (e) {
  var isTypeError = (e instanceof TypeError);
}
eq(isTypeError, true);
// returns type error
```
- does not work on arguments object
```javascript
var testArgs;
function argsTest(a, b, c) {
  testArgs = arguments;
}
argsTest(1, 2, 3);
try {
  push(testArgs,4)	
} catch (e) {
  var isTypeError = (e instanceof TypeError)
}
eq(isTypeError, true);
// returns type error
```

# Step 3
## Prototype Implememtation
```javascript
function push(arraylike, ...elementN) {
  if (typeof(arraylike) === 'string') {
    throws new error;
  }
  if (Object.prototype.toString.call(arraylike) === '[object Arguments]') {
    throw new error;
  }
  if (!arraylike.hasOwnProperty(length)) {
    arraylike.length = 0;
  }
  if (!Number.isInteger(arraylike.length)) {
    arraylike.length = parseInt(arraylike.length);
    if (isNaN(arraylike.length)) { arraylike.length = 0; }
  }
  var endOfArraylike = arraylike.length;
  var indexesToAdd = endOfArraylike + elementN.length;
  var elementNPosition = 0;

  for (var i = endOfArraylike; i < indexesToAdd; i++) {

    arraylike[i] = elementN[elementNPosition];
    elementNPosition++;
  }
  arraylike.length = i; // h
  return i;
}
```
## Function
push will mutute arraylike and add elementN elements to the end of the arraylike

## Syntax
push(arraylike, ...elementN)

## Parameters
arraylike
elementN

## Returns
length of new arraylike object

Requirements
- it should add elements from elementsN in order to the end of arraylike
- it should work with any arraylike elements with the length property

- it should push new elements based on the length property to determine where to start inserting
- if length can't be converted to number, it should set length to 0
- if length doesn't exist, it should create the property

- if using a string arraylike, it should throw a TypeError
- if usig an arguments object, it should throw a TypeError