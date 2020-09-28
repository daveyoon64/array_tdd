# Array.prototype.reverse() Step 1
reverse() reverses array in place

## Example
```javascript
const array1 = ['one', 'two', 'three'];
console.log('array1:', array1);
// expected output: "array1:" Array ["one", "two", "three"]

const reversed = array1.reverse();
console.log('reversed:', reversed);
// expected output: "reversed:" Array ["three", "two", "one"]

// Careful: reverse is destructive -- it changes the original array.
console.log('array1:', array1);
// expected output: "array1:" Array ["three", "two", "one"]
```
## Syntax
a.reverse()

## Returns
reversed array

## Description
- tranposes array elements or array in-place
- mutates array
- returns reference to reversed array
- generic enough for call
- generic enough for apply
- if it doesn't have a length property, it will not work
- reversing an arraylike object
```javascript
const a = {0: 1, 1: 2, 2: 3, length: 3};
console.log(a); // {0: 1, 1: 2, 2: 3, length: 3}
Array.prototype.reverse.call(a); //same syntax for using apply()
console.log(a); // {0: 3, 1: 2, 2: 1, length: 3}
```
# Step 2
## Prototype Implementation
```javascript
function reverse(arraylike) {
  
  var leftPtr = 0;
  var rightPtr = arraylike.length - 1;

  while (leftPtr <= rightPtr) {
    var temp = arraylike[leftPtr];
    arraylike[leftPtr++] = arraylike[rightPtr];
    arraylike[rightPtr--] = temp;
  }
  return arraylike;
}
```
## Description
reverse() reverse array in-place

## Function Signature
reverse(arraylike)

## Parameters
arraylike

## Returns
reference to arraylike

## Requirements
- reverse mutates the arraylike give as parameter
- returns reference to reversed array
- if arraylike is empty, it returns []
- generic enough for call
```javascript
const a = {0: 1, 1: 2, 2: 3, length: 3};
reverse.call(null, a); 
console.log(a); // {0: 3, 1: 2, 2: 1, length: 3}
```
- generic enough for apply
```javascript
const a = {0: 1, 1: 2, 2: 3, length: 3};
reverse.apply(null, [a]); 
console.log(a); // {0: 3, 1: 2, 2: 1, length: 3}
```
- if it doesn't have a length property, it will not work
```javascript
var a = {0: 1, 1: 2, 2: 3};
reverse.call(a);
// nothing happens to arraylike
```
- reversing an arraylike object
```javascript
const a = {0: 1, 1: 2, 2: 3, length: 3};
reverse.call(a); 
console.log(a); // {0: 3, 1: 2, 2: 1, length: 3}
```
- can't reverse a string
- can't reverse an arguments object

# Step 3
## Prototype Implementation
```javascript
function reverse(arraylike) {
  var leftPtr = 0;
  var rightPtr = arraylike.length - 1;

  while (leftPtr <= rightPtr) {
    var temp = arraylike[leftPtr];
    arraylike[leftPtr++] = arraylike[rightPtr];
    arraylike[rightPtr--] = temp;
  }
  return arraylike;
}
```
## Description
reverse() reverse array in-place

## Function Signature
reverse(arraylike)

## Parameters
arraylike

## Returns
reference to arraylike

## Requirements
- it should mutate the arraylike by reversing the order of its elements
- it should return a reference to reversed array
- if arraylike is empty, it should return []
- it should be generic enough for call
- it should be generic enough for apply
- if it doesn't have a length property, it should not work
- it should reverse an arraylike object
- it should not reverse a string
- it should not reverse an arguments object