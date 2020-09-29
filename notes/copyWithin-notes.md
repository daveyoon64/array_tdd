# copyWithin() Step 1
copyWithin() shallow copies part of array to another location in same array
does not modify length

## Syntax
arr.copyWithin(target(, start[, end]]))

## Parameters
- target: position in element where we want to start copying
- start: where to start copying elements from
- end: when it copies up to, but does not include end


## Description
- copyWithin starts at target, then will copy from array position 0 to array.length
- if start, copyWithin start at target, then will copy from start to array.length
- if start and end, copyWithin starts at target, then will copy from start to end (max of arr.length)

- if target < 0, calculate position using array.length + target
- if target > arr.length, copy nothing
- if target is positioned after start, copy sequence will be trimmed to fit arr.length

- if start < 0, calculate position using array.length + start
- if no start, start at index 0

- if end < 0, calculate position using array.length + end
- if no end, copyWithin will copy until the last index (default to arr.length)

- copyWithin is generic so it should work with call
- copyWithin is generic so it should work with apply

## Returns
reference to modified array

## Notes
- the target is just where we start
- the actual loop will be determined by start and end
- experimenting, it stops when the end of array is reached from the target or inner loop
- reading specs again, it's a shallow copy
  - so i'm thinking of creating a subarray with target position...
  - THEN iterate it from start to end
[1, 2, 3, 4, 5]
- hmm, the problem is that if target is greater than start, like
copyWithin([1, 2, 3, 4], 1, 0)
// current: [1, 1, 1, 1]
  - it's currently using the overwritten value
  - so if target > start, make a shallow copy, and then use that

## Step 2
# Prototype Implementation
```javascript
function copyWithin(array, target, start, end) {

  var copyStartPtr = start;
  var copyEndPtr = end;  
  // prevents overwrite when target > start
  var subArray = array.slice(copyStartPtr, copyEndPtr);

  var j = 0;
  for (var i = target; i < array.length; i++, j++) {
    array[i] = subArray[j];
    if (i === array.length - 1) break;
    if (j === subArray.length - 1) break;
  }
  return array;
}
```
## Definition
copyWithin() shallow copies subarray in same array at target location

## Parameters
- array
- target
- start
- end

## Returns
reference to mutated array

## Requirements
- Given target, it should shallow copy from position 0 to array.length, starting at target position


- If given start, it should shallow copy from position start to array.length, starting at target position
- If given start and end, it should shallow copy from position start to end, starting at target position (max of arr.length)

- If target < 0, it should calculate position using array.length + target, then shallow copy starting at target position
- If target > array.length, it should copy nothing
- If target is positioned after start, it should copy a sequence trimmed to fit array.length

- If start < 0, it should calculate position using array.length + start
- If no start, it should start at index 0

- If end < 0, it should calculate position using array.length + end
- If no end, it should copy until the last index (default to arr.length)

- It should be generic to work with call
- It should be generic to work with apply

- It should not work with a string
- It should not work with an arguments object

