# Array.prototype.sort() Step 1
sort() sorts elements of array in place and returns reference to mutated, sorted array
default is ascending
converts elements into string, then compares UTF-16 code unit values

## Syntax
arr.sort([compareFunction[, firstEl[, secondEl]]])

## Parameters
compareFunction: specifies function for sort order
  - if omitted, elements converted to strings, then sorted according to unicode
  - firstEl: first element for comparison
  - secondEl: second element for comparison

## Return
reference to sorted array (no copy)

## Description
- if no compare function, elements converted to strings, then sorted according to unicode
  - banana vs cherry (unicode, 80 comes before 9)
  - all undefined elements are sorted to end of array

- If compareFunction is supplied, all non-undefined array elements are sorted according to the return value of the compare function
  - undefined elements go to the back and make no call to compareFunction

- compareFunction(a, b)
  - if returns < 0, sort a to index lower than b (a before b)
  - if returns 0, a and b are unchanged with respect to each other, but still sorted vs everything else
  - if returns > 0, sort b to index lower than a (b before a)
  - must always return same value when give specific pair . if inconsistent, sort order is undefined

- you can sort objects using the value of their properties
- you can compare non-ASCII characters and sort them
- you can sort using map function

## Notes
- getting caught up on the sorting aspect, so I just threw it into my list of stuff to comeback to
- I chose an in-place quicksort
- I need to be able to use the callback provided, not the default "ascending" solution
```javascript
var numbers = [4, 2, 5, 1, 3];
sort(numbers, function(a, b) {
  return a - b;
});
console.log(numbers);
// [1, 2, 3, 4, 5]
```
- the whole -1, 0, 1 thing is super weird... do I have to? lol
- something like this
```javascript
function compare(a, b) {
  if (a < b) {
    return -1;
  }
  if (a > b) {
    return 1;
  }
  // a must be equal to b
  return 0;
}
```
- stop and thought about it some more, making diagrams..
- i'm caught up with trying to fit what javascript does with my cookie cutter quicksort
```javascript
function sort(array, callback) { 
  function divide(start, end) {
    if (start >= end) { return; }
    let mid = start;
    for( let i = start; i < end; i++) {
      if (array[i] < array[end]) {
        [array[i], array[mid]] = [array[mid], array[i]];
        mid++;
      }
    }
    [array[mid], array[end]] = [array[end], array[mid]];
    divide(start, mid - 1);
    divide(mid + 1, end);
  }
  divide(0, array.length - 1);
  return array;
}
```

# Step 2
## Prototype Implementation
```javascript
function sort(array, callback) {
  // ???
  return array;
}
```
## Description
sort()sorts array elements in place and returns reference to mutated array

## Function Signature
sort(array, callback)

## Parameters
array
callback

## Callback Parameters
elementA, elementB: the 2 elements we're comparing in the callback

## Return
Reference to sorted array

## Requirements
- if no callback, it should convert elements to string, then sort according to unicode
- if callback, array elements are sorted according to the return value of the callback
- it should sort all undefined elements to the end of the array

- if callback returns -1, elementA comes before elementB
- if callback returns 0, elementA and elementB are unchanged
- if callback returns 1, elementB comes before elementA

- it should be able to sort objects using property values
- it should handle non-ASCII characters
- it should be able to sort using Array.prototype.map()
