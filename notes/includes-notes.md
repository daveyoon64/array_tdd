# Array.protoytype.includes()
includes() determines contains a certain value

returns boolean

## Example
```javascript
const pets = ['cat', 'dog', 'bat'];

console.log(pets.includes('cat'));
// expected output: true

console.log(pets.includes('at'));
// expected output: false
```

## Syntax
arr.include(valueToFind[, fromIndex])

## Parameters
valueToFind: what we're searching for
fromIndex: position on where to start looking for the element

## Description
- includes() is case-sensitive
- if fromIndex is positive, use that number
- if fromIndex is negative, user arr.length + fromIndex
- "using the absolute value of fromIndex as the number of elements from the end of the array at which to start the search"
	- i haven't the slightest clue what they're trying to say
- if there's no fromIndex, the default is 0

## Return value
- a boolean, true if valueToFind is found in array
- values of zero are equal (-0, +0, 0 are all the same, but false is not the same as zeroes)
	- uses sameValueZero algorithm
	- if they're all equal, do I have to test for them? what's the purpose of this?

## More Examples
- if fromIndex >= array.length, false is returned. array is not searchd
- if fromIndex is negative, it computes the reverse position as array.length + fromIndex
	- if it's still negative, then the whole array is searched 
- it's a generic method, not requiring a this. therefore, it should work for other kinds of objects
	- read about Array.from()
```javascript
(function() {
  console.log(Array.prototype.includes.call(arguments, 'a'))  // true
  console.log(Array.prototype.includes.call(arguments, 'd'))  // false
})('a','b','c') 
```

# Step 2
Prototype Implementation
```javascript
function includes(array, valueToFind, fromIndex) {
  var startIndex = 0;

  if (fromIndex) {
    startIndex = fromIndex;
    if (fromIndex < 0) {
      startIndex = array.length + fromIndex;
    }
  }
  for (var i = startIndex; i < array.length; i++) {
    if (array[i] === valueToFind) {
      return true;
    }
  }
  return false;
}
```

## Function Signature
includes(arraylike, valueToFind, fromIndex)

## Parameters
array
valueToFind
fromIndex

## Returns
- true if valueToFind is found
- false if valueToFind is not found

## Requirements
- when comparing strings and characters, includes() is case-sensitive
```javascript
include('DAVE', 'a');
// return false
```
- given an array and a valueToFind, if it finds valueToFind, it'll return true
- given an array and a valueToFind, if it does not find valueToFind, it'll return false
- if there's a fromIndex, set position there, then search for valueToFind
- if fromIndex is negative, use array.length + fromIndex, set position, then search for valueToFind
- if fromIndex is still negative after calculating index above, search entire index
- if there's no fromIndex, start from 0
- if fromIndex >= array.length, false is returned and array is not searched
- include() should work for all array-like objects
```javascript
include('water', 'r', 1);
// return true
```


# Step 3
## Prototype Implementation
```javascript
function includes(arraylike, valueToFind, fromIndex) {
  // need to use Array.from() to prep the arraylike
  var startIndex = 0;

  if (fromIndex) {
    startIndex = fromIndex;
    if (fromIndex < 0) {
      startIndex = arraylike.length + fromIndex;
    }
  }
  for (var i = startIndex; i < arraylike.length; i++) {
    if (arraylike[i] === valueToFind) {
      return true;
    }
  }
  return false;
}
```

## Function Signature
includes(arraylike, valueToFind, fromIndex)

## Parameters
arraylike
valueToFind
fromIndex

## Returns
- true if valueToFind is found
- false if valueToFind is not found

## Requirements
- it should return true if it finds valueToFind in array
- it should return false if it does not find valueToFind in array

- if no fromIndex, it should search the entire array
- if fromIndex, it should start searching the array at fromIndex
- if fromIndex < 0, calculate the index as array.length + fromIndex, then it should start searching the array at calculated index
- if fromIndex is still negative after calculated index, it should search the entire array
- if fromIndex >= array.length, it should return false and the array is not searched

- it should be case-sensitive
- it should work for all array-like objects

## Notes
- Thinking about includes() and it's ability to handle any array-like data structure. It makes sense if the tests themselves implicitly use different data structures to ensure they work.