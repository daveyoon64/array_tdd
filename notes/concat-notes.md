# Array.prototype.concat Step 1

concat() used to merge 2 or more arrays
it does not change the existing arrays

## Example
```javascript
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);

console.log(array3);
// expected output: Array ["a", "b", "c", "d", "e", "f"]
```
## Returns
A new array instance consisting of merged elements

## Syntax
const new_array = old_array.concat(array1, array2 ... arrayN)
It will merge all arrays
If no value parameters given, will return a shallow copy of the original array

## Description
- the parameters are merged together in order called
- the array, then the elements of the array
- within the array
	- if elements are array or element, will add the element to the new array
	- it does not recurse into nested array args, but add the array as an element
- returns a shallow copy of the same elements combined from the original arrays
	- copies object references into new array
	- original and new array refer to same object (if reference object is modified, the changes come down to both the new and original arrays, including elements that are arrays)
	- data types (strings, numbers, booleans): concat copies the values of strings and number into the new array


# concat Step 2
## Prototype Implementation
```javascript
// primary will be the starting point for the array merge
function concat(primaryArray, ...arrays) {
  // building new array to return
  // since concat does not change existing array, make a copy
  // alternate way, which one fits the specs?
  // var newArray = Object.assign([], primaryArray);
  var newArray = [...primaryArray]; 

  // starting with primaryArray, 
  arrays.forEach(function(array) {
    for (var i = 0; i < array.length; i ++) {
      newArray.push(array[i]);
    }
  })
  console.log(newArray);
  return newArray;
}
```
concat(primaryArray, ...arraysN)

returns a new array instance

Description
- merges 2 or more arrays
 ```javascript
var arr = [1, 2];
var arr2 = [3. 4];
var test = concat(arr, arr2);
// returns [1, 2, 3, 4]
```

- if no additonal array parameters, return a shallow copy of primaryArray
```javascript
var arr = [1, 2];
var test = concat(arr);
// returns a shallow copy of arr
```

- it will not recurse into nested arrays, it will simply add the array as an element
```javascript
var arr = [1, [2]];
var arr2 = [[2, 3, 4], [3, 6];
var test = concat(arr, arr2);
// test is [1, [2], [2, 3, 4], [3, 6]];
```

- concat() will return a shallow copy of all the elements combined from arraysN
- original array and new array will refer to the same object reference
- if referenced object is modified, changes should be visible in the new and original arrays
```javascript
var arr = [1, [2]];
var arr2 = [[2, 3, 4], [3, 6];
var test = concat(arr, arr2);
// test is [1, [2], [2, 3, 4], [3, 6]];

arr[1].push(15);
console.log(test);
// test is now [1, [2, 15], [2, 3, 4], [3, 6]];
```

- data types such as strings, numbers, and boolearn (primitives) will be copied into new array
```javascript
var arr = [1, 'dave'];
var arr2 = [true];
var test = concat(arr, arr2);
// test is [1, 'dave', true]
```
- mutating anything on the array instance does not affect source arrays
```javascript
var arr = [1, 2];
var arr2 = [3, 4];
var test = concat(arr, arr2);
test[0] = 999;
test[2] = 777;
// test is [999, 2, 777, 4]
// arr is still [1, 2], arr2 is still [3, 4]
```

# Step3
## Prototype Implementation
```javascript
// primary will be the starting point for the array merge
function concat(primaryArray, ...arrays) {
  // building new array to return
  // since concat does not change existing array, make a copy
  // alternate way, which one fits the specs?
  // var newArray = Object.assign([], primaryArray);
  var newArray = [...primaryArray]; 

  // starting with primaryArray, 
  arrays.forEach(function(array) {
    for (var i = 0; i < array.length; i ++) {
      newArray.push(array[i]);
    }
  })
  console.log(newArray);
  return newArray;
}
```

## Function Signature
concat(primaryArray, ...arraysN)

## Returns
returns a new array instance

## Requirements
- it should accept a primaryArray as the 1st argument
- if no arraysN, then concat returns a new array instance of primaryArray
- if arraysN, it should accept an number of arrays as the arrays parameter
- if arraysN, it should merge the arrays, left to right, into the new single, array instance
- it should not recurse into nested arrays, it will just add the array as an element
- the return value is a shallow instance, so the original array and new array share object references
- it should accepts data types such as strings, numbers, and booleans
- if modifying the new array instance, it should not mutate the source arrays