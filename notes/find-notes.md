# Step 1
## summary
find() returns value of 1st element that satisfies callback function

## example
var array = [1, 2, 4, 6];
var num = find(array, function(number) {
  return number > 2;
});
// num = 4 (the first one found!)

## parameters
arr.find(callback(element(index, array), this)
so as a function... probably...
find(array, callback(numberToTest, index, originalArray), optionalThis)

## return
value of first element that satisfies the callback
otherwise undefined

## specs
executes callback for each index of array until truthy returned
if so, find returns immediately, else undefined

callback called on EVERY array index, so holes/spare arrays don't matter

does not mutate array on it being called, but the callback can
	if so
		el processed by find are set BEFORE 1st invocation

	therefore
		callback will not visit elements added after call to find begins
		if existing, unvisited element of array is changed by cb, value found by find
			will be the updated one
		deleted elements are still visited

# Step 2
prototype implementation
```javascript
function find(array, callback, optionalThis) {
  var firstFoundNumber = 0;
  for (var i = 0; i < array.length; i++) {
    firstFoundNumber = callback(array[i], index, originalArray);
    if (firstFoundNumber) {
      return firstFoundNumber;
    }
  }
}
```

## function declaration
find(array, callback, optionalThis)

callback(currentElement, currentIndex, originalArray)

returns single value (that passes cb test)
else returns undefined

calls on every array index

## test cases
Case A: find goes through an array, returns first value that passes
```javascript
[7,2,5,45].find(number=>number>40)
returns 45
```
```javascript
Case B: find goes through an array with holes, returns first value that passes
[7,,2,,undefined,45].find(number=>number>40)
returns 45
```
```javascript
Case C: find goes through array, no element passes cb, returns undefined
[7,2,5,45].find(number=>number>100)
returns undefined
```

## Edge Cases:
if cb changes the array, new elements will be ignored
```javascript
var array = [1, 3, 5, 7, 9]
find(array, function(number, index) {
  array[index] *= 2;
  if (number > 17) {
    return number;
  }
}) // returns undefined, because array[4] will be 18, but find() will only test 9 and not 18
```
```javascript
if cb adds elements to the array after find() starts, it will not visit those elements
var array = [1, 3, 5, 7, 9]
find(array, function(number, index) {
  array.push[25];
  if (number > 17) {
    return number;
  }
}) // returns undefined, even though 25 was pushed to end
```
if an element already exists, is changed by the cb, and unvisisted, find() will use the updated value
```javascript
var array = [1, 3, 5, 7, 9]
find(array, function(number, index) {
  array[4] = 18;
  if (number > 17) {
    return number;
  }
}) // returns 18
```

if an element is deleted, find() will still visit that element
```javascript
var array = [1, 3, 5, 7, 9]
find(array, function(number, index) {
  if (index === 0) {
    delete array[4];
  }
  if (number > 8) {
    return number;
  }
}) // returns undefined, because it was deleted]
```

# Step 3
## prototype implementation
```javascript
function find(array, callback, optionalThis) {
  var firstFoundNumber = 0;
  for (var i = 0; i < array.length; i++) {
    firstFoundNumber = callback(array[i], index, originalArray);
    if (firstFoundNumber) {
      return firstFoundNumber;
    }
  }
}
```
## function signature
```javascript
find(array, callback[, initialValues], optionalThis)
```
callback parameters
	currentElement
	currentIndex
	originalArray

returns
	first value that passes callback else undefined

## requirements
it should go through the array and return the first value that passes the callback
it should go through the array and return undefined if no element passes the callback
if array has holes, it should go through the array and return the first value that passes the callback

after find starts, if the callback mutates the array, the new values should be ignored
after find starts, it the callback adds elements to the array, the new pushed value should be ignored

if an element already exists, is mutated by the callback, and is unvisited, it should find the mutated value
if an element that exists is deleted, it should still visit that element
