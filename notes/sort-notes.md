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
// basic quicksort
function sort(array, callback) { 

  function qsswap(start, pivot) {
    if (start >= pivot) { return; }
    let wall = start;
    for ( var i = start; i < pivot; i++) {
      if (callback(array[i], array[pivot])) {
        [array[i], array[wall]] = [array[wall], array[i]];
        wall++; 
      }
    }
    // now we want to move our wall it it's proper place!
    [array[wall], array[pivot]] = [array[pivot], array[wall]];
    // then recurse both sides!
    qsswap(0, wall - 1);
    qssswap(wall, pivot);
  }
  qsswap(0, array.length - 1);
  return array;
}
```
- the wall acts as the barrier for the values that are less than or greater than the pivot
- the wall is incremented whenever a lower value is found
- I'm stuck because I keep on using quicksort. Stopping that and thinking from the ground up
- Finally figured it out. Somehow it got into my head that a + b should be descending, but it would never return less than 0. I finally stopped, questioned it, tested it, and saw that even the native sort doesn't deal with a+b
- I need to challenge assumptions faster 
- if array is mixed, a callback is provided, built-in sort will not even bother
 
# Step 2
## Prototype Implementation
```javascript
function sort(array, callback) {
  var isNaNCheck = false;
  var defaultCallback = function(a, b) {
    return a.toString() < b.toString();
  }

  function qsswap(start, pivot) {
    if (start >= pivot) { return; }
    let wall = start;

    for ( var i = start; i < pivot; i++) {
      if (array[i] === undefined) {
        [array[i], array[pivot]] = [array[pivot], array[i]];
        while (array[pivot] === undefined) { pivot--; }
      }
      
      if (!callback) {
        while (array[pivot] === undefined) { pivot--; }
        if (defaultCallback(array[i], array[pivot])) {
          [array[i], array[wall]] = [array[wall], array[i]];
          wall++;
        }
      } else {
        while (array[pivot] === undefined) { pivot--; }
        isNaNCheck = callback(array[i], array[pivot]);
        if (Number.isNaN(isNaNCheck)) {
          isNaNCheck = true;
          break;
        }
        if (typeof isNaNCheck === 'boolean') {
          isNaNCheck = true;
          break;
        }
        if (callback(array[i], array[pivot]) <= 0) {
          [array[i], array[wall]] = [array[wall], array[i]];
          wall++; 
        }
      }
    }

    if (!Number.isNaN(isNaNCheck) || !isNaNCheck) {
      // now we want to move our wall it it's proper place!
      [array[wall], array[pivot]] = [array[pivot], array[wall]];
      // then recurse both sides!
      qsswap(0, wall - 1);
      qsswap(wall, pivot);
    }
  }
  qsswap(0, array.length - 1);
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
```javascript
sort([49, 20, 5, 1000, 4100])
// returns [1000, 20, 4100, 49, 5]
```
- if callback, array elements are sorted according to the return value of the callback
```javascript
sort([49, 20, 5, 1000, 4100], function(a, b) { return a - b})
// returns [5, 20, 49, 1000, 4100]
```
- it should sort all undefined elements to the end of the array
```javascript
sort([49, 20, 5, undefined, 4100], function(a, b) { return b - a})
// returns [4100, 49, 20, 5, undefined]
```
- if callback, and elementA - elementB is less than 0, swap elementA and elementB.
```javascript
sort([20, 49, 5, 4100, 1000], function(a, b) { return a - b})
// returns [5, 20, 49, 1000, 4100]
```
- if callback, and elementB - elementA is less than 0, swap elementB and elementA. 
```javascript
sort([2000, 5, 346, 16], function(a, b) { return b - a})
// returns [2000, 346, 16, 5]
```
- if callback returns 0, elementA and elementB do not swap
```javascript
sort([2000, 5, 2000], function(a, b) { return b - a})
// returns [2000, 2000, 5]
```
- it should be able to sort objects using property values
```javascript
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 }
];
sort(items, function(a, b) {
  var nameA = a.name.toUpperCase();
  var nameB = b.name.toUpperCase();
  if (nameA < nameB) {
    return -1;
  }
  if (nameA > nameB) {
    return 1;
  }
  return 0;
});
// 0: {name: "And", value: 45} 1: {name: "Edward", value: 21} 2: {name: "Magnetic", value: 13}
```
- it should handle non-ASCII characters
```javascript
var items = ['réservé', 'premier', 'communiqué', 'café', 'adieu', 'éclair'];
sort(items, function (a, b) {
  return a.localeCompare(b);
});
```

- it should be able to sort using Array.prototype.map()
```javascript
var list = ['Delta', 'alpha', 'CHARLIE', 'bravo'];

var mapped = list.map(function(el, i) {
  return { index: i, value: el.toLowerCase() };
})
sort(mapped, function(a, b) {
  if (a.value > b.value) {
    return 1;
  }
  if (a.value < b.value) {
    return -1;
  }
  return 0;
});
// 0: {index: 1, value: "alpha"} 1: {index: 3, value: "bravo"} 2: {index: 2, value: "charlie"} 3: {index: 0, value: "delta"}
```