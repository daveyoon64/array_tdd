# Array.prototype.splice() Step 1
splice() changes contents of array by removing or replacing existing elements, and/or adding new elements in-place

## Example
```javascript
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
// inserts at index 1
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May');
// replaces 1 element at index 4
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "May"]
```
## Syntax
let arrDeletedItems = array.splice(start[, deleteCount[, item1[, item2[, ...]]]])

## Parameters
start: where to start
deleteCount: how many to delete
item1, item2, ...: how many elements we're adding

## Return
an array containing the deleted items
if only one element, return array of 1 element
if no elements removed, return empty array

## Description
- if number of elements to insert differ from elements being remove, update the array's length

- if start > array.length, start will be set at length of array
  - no element is deleted but will add them
  - if start < 0, it starts from that many elements from end of array (array.length + start)
  - if calculated index (array.length + start) is still < 0, index set to 0

- if no deleteCount or if value >= array.length - start (>= num of elements left in array), then those elements will be deleted
- if deleteCount < 0, no elements are removed

- ...items are items you add to the array, beginning from start

## Notes
```javascript
items = [4, 45, 12]
deleteCount = 1
[1, 3, 5, 2, 7]
       ^
       |
       start
[1, 3, 5, empty, 7]
       ^
       |
       start
[1, 3, 5, 4, 45, 12, 7] //reindex each for for each element in items
       ^
       |
       start
```
OR just index once!
```javascript
[1, 3, 5, 2, 7]
       ^
       |
       start
[1, 3, 5, 2, 7, empty empty] //add enough space (items.length - deleteCount)
       ^
       |
       start
[1, 3, 5, empty, empty, 2, 7] //reindex to shift everything to the right
       ^
       |
       start
[1, 3, 4, 45, 12, 2, 7] // then just assign values
       ^
       |
       start
// if deleteAt is 0, we still need to add one empty!
```
```javascript
splice([1, 2, 3, 4], 1, 2, 5, 6, 7, 8, 9)
(6) [1, 5, 6, 7, 8, 9] <---- WRONG
// This should be [1, 5, 6, 7, 8, 9, 4]
// it gets here as [1, empty, empty, 4]
// but itemsToDelete has 5 elements
// assign it whatever in items
// deleted.length is 2
// items.length is 5
// We need to add 3 more items.length - deleted.length
// but it has to be adjacent to the other empties
```
# Step 2
## Prototype Implementation
```javascript
function splice(array, start, deleteCount, ...items) {
  var startPos = start;
  var elementsToDelete = array.length;
  var deleted = [];

  if (start > array.length) {
    startPos = array.length;
  } else if (start < 0) {
    startPos = array.length + start;
  }
  if (startPos < 0) {
    startPos = 0;
  }

  if (deleteCount) {
    elementsToDelete = deleteCount;
    if (elementsToDelete >= array.length - start) {
      elementsToDelete = array.length - start;
    }
  }
  if (elementsToDelete < 0) {
    elementsToDelete = 0;
  }
  // delete from start to start + deleteCount
  for (var i = startPos; i < startPos + elementsToDelete; i++) {
    deleted.push(array[i]);
    delete array[i];
  }
  // if there are more items than deleteCount:
  // 1. we first push on undefined
  // 2. reindex values by push all truthys to the right of array
  if (items.length > deleted.length) {
    for (var j = 0; j < items.length - deleted.length; j++) {
      array.push(undefined);
    }
  }
    // move everything from startPos to the right
  var slowPtr = array.length - 1; // only moves forward when condition met
  var fastPtr = array.length - 1; // our traversal pointer
  // move fastPtr to first truthy value
  while (array[fastPtr] === undefined) {
    fastPtr--;
  }
  while (fastPtr >= startPos) {
    array[slowPtr--] = array[fastPtr--];
  }

  for (var k = 0, j = startPos; k < items.length; j++, k++) {
    array[j] = items[k];
  }

  if (array[0] === undefined) {
    // remove all undefineds left of the value
    var slowPtr = 0; // only moves forward when condition met
    var fastPtr = 0; // our traversal pointer
    // move fastPtr to first truthy value
    while (array[fastPtr] === undefined) {
      fastPtr++;
    }
    while (fastPtr < array.length) {
      array[slowPtr++] = array[fastPtr];
      array[fastPtr++] = undefined;
    }
  }
  // remove all undefineds right of the values
  if (array.includes(undefined)) { 
    var arrayEnd = array.length - 1;
    while (array[arrayEnd] === undefined) {
      arrayEnd--;
    }
    array.length = arrayEnd + 1;
  }
  return array;
}
```

## Definition
splice() changes contents of array by removing or replacing existing elements, and/or adding new elements in-place

## Syntax
splice(array, start, deleteCount, ...items)

## Parameters
array
start
deleteCount
...items

## Returns
Array with deleted items (might need delete?)

## Requirements
- if there is no start, it should return a []
- If start and deleteCount removes 1 element, it should return an array with that 1 element
- If start is 0 and no deleteCount, it should remove all elements from array

- if start > array.length, it should be set to array.length
- if start < 0, it should calculate array.length + start and use the calculated index
- if start < 0 after calculation, it should set start to 0

- if deleteCount is larger than the remaining array elements, it should set deleteCount to array.length - start
- if deleteCount < 0, it should set deleteCount to 0

- if given start, it should remove elements from start to the end of the array.
- if given start and deleteCount, it should remove elements from start to start + deleteCount
- if given start and deleteCount is 0, it should remove no elements
- it should return an array with all deleted elements
- it should modify the array in-place

- if given start, deleteCount, and items, it should remove elements from start to start + deleteCount and insert items elements
- if given start and deleteCount within the array, it should remove elements from start to start + deleteCount, then insert items elements, and not affect existing elements
- if start > array.length, it should not remove any elements and append items to the array