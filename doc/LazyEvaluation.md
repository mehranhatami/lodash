# Lazy Evaluation

This document will walk you through the whole steps taken to setup the lazy evaluation in lodash.

# First
In the first step what you should do is simply wrapping the main lodash function around the desired collection or object which is being under evaluation lazily.

```javascript
var collection = [{ val: 11 }, { val: 22 }, { val: 33 }, { val: 4 }, { val: 7 }];
var wrapped = _(collection);
```
And what it will essentially does is creating a new `LodashWrapper` object like:

```javascript
var wrapped = new LodashWrapper(collection);
```

in this state wrapped Object will be like:

```javascript
{
  __wrapped__   : collection,
  __actions__   : [],
  __chain__     : false
}
```

along with all the other methods and attributes in `LodashWrapper.prototype` which itself gets inherited from `baseLodash`:

```javascript
LodashWrapper.prototype = baseCreate(baseLodash.prototype);
```

# Second
Now that you have managed to create a new object you could call it on and pass the callback function, like:

```javascript
function func1(obj) {
  return obj.val;
}
var mappedObj = wrapped.map(func1);
```

This is the very point that things get confusing a bit. Do you think `mappedObj` should be an array as the result of the `wrapped.map(...)`.

__But apparently it is not__!! It is returning us another instance of `LodashWrapper`. Lazy eveluation is now implemented in this functions:

```javascript
[
  'dropWhile','filter','map','takeWhile',
  'drop','dropRight','dropRightWhile',
  'take','takeRight','takeRightWhile',
  'first','last','initial','rest','pluck',
  'where','compact','reject','slice','toArray'
]
```
Any of the items in the list will does the same thing, returning an instance of `LodashWrapper` instead of returning actual values.
