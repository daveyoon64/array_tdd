# Array.prototype.join Step 1

join() creates and returns a new string by concatenating all elements in array (or arraylike)
you can specifiy a separator like comma

if it only has one item, then return that one item, no seperator

## Example
```javascript
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```
## Syntax
arr.join([seperator])

## Parameters
- separator: the string we want to use when separating the elements
- separator is converted to string
- if omitted, it will default to ","
- if empty string, no characters are used

## Return
- a string will all array elements joined
- if arr.length is 0, return empty string

## Description
i- f an element is undefined, null, or an empty array, it is converted to an empty string
```javascript
[1, 'a', null, 'c', undefined, 'd', [], 'e',,,,,,,,'zzzz'].join('')
"1acdezzzz"
```
- 1st obs: it appears to simple skip null, undefined, [], and holes
- 2nd obs: not true, if you indicate a separator, you see that it does process it

# Step 2
## Prototype Implementation
```javascript
function join(arraylike, separator) {
  var newArray = Array.from(arraylike);
  var newString = '';

  function arrayHelper(array, pos){
    if (pos > array.length - 1) { return; } // base case
    if (pos !== 0) {
      separatorHelper(separator);
    }
    newString += array[pos];
    arrayHelper(array, pos + 1);
  }
  function separatorHelper(separator) {
    if (separator === '') {
      return;
    } else if (!separator) {
      newString += ',';
    } else {
      newString += separator;
    }
  }
  for (var i = 0; i < newArray.length; i++) {
    if (i !== 0) {
      separatorHelper(separator);
    }
    if (Array.isArray(newArray[i])) {
      arrayHelper(newArray[i], 0);
    } else {
      newString += newArray[i];
    }
  }
  return newString;
}
```
## Function Signature
join(arraylike, separator)

## Parameters
arraylike
separator

## Return
String

## Requirements
if there is an array as an element within the parent element, enter it and merge each element
```javascript
join(['dave', ['jim', 'bill'], ['kim', ['lim']]]);
// returns "dave,jim,bill,kim,lim"
```
if there is a blank array as an element within the parent element, treat it as a string
if the iterable has items, then append all items into a new array and return it
if separator, use the separator in-between each merged element
if separator is equal to a blank string, use no separator in-between each merged element
```javascript
join(['dave', ['jim', 'bill'], ['kim', ['lim']]]);
// returns "davejimbillkimlim"
```
if no separator, use the ',' character in-between each merged element
if array.length is 0, return empty string
```javascript
join([]])
// return ''
```
if an element is undefined, null, or an empty array, it is converted to an empty string
```javascript
join(['', undefined, null, []])
// return ,,,,
```

# Step 3
## Prototype Implementation
```javascript
function join(arraylike, separator) {
  var newArray = Array.from(arraylike);
  var newString = '';
  // helper inner function for array within array
  function arrayHelper(array, pos){
    if (pos > array.length - 1) { return; } // base case
    if (pos !== 0) {
      separatorHelper(separator);
    }
    newString += array[pos];
    arrayHelper(array, pos + 1);
  }
  // helper inner function to deal with separator cases
  function separatorHelper(separator) {
    if (separator === '') {
      return;
    } else if (!separator) {
      newString += ',';
    } else {
      newString += separator;
    }
  }

  for (var i = 0; i < newArray.length; i++) {
    if (i !== 0) {
      separatorHelper(separator);
    }
    if (Array.isArray(newArray[i])) {
      arrayHelper(newArray[i], 0);
    } else if(!newArray[i]) {
      continue;
    } else {
      newString += newArray[i];
    }
  }
  return newString;
}
```
## Function Signature
join(arraylike, separator)

## Parameters
arraylike
separator

## Return
String

## Requirements
- it should merge all elements in the array and return a single string
- if there are arrays as elements, it should flatten those array and merge the arrays elements into the string

- if separator, it should use the operator in-between each merged element.
- if separator is equal to a blank string, it should use no separator when merging elements
- if no separator, it should the , character in-between each merged element.

- if array.length is 0, it should return a blank string
- if there is a blank array as an element, it should treat it as a blank string
- if an element is undefined, null, or an empty array, it should treat it  as a blank string


