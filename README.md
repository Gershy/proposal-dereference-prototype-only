# TC39 Proposal: Dereference from Prototype Only

## Status

**Stage**: 0
**Champion**: None yet

## Summary

Proposal for a new operator which performs property dereference, but ignores the surface level ("own" properties) of the property chain.

Example:
```
const obj = { a: 1 };
console.log(obj..a);        // undefined
console.log(obj..toString); // [Function: toString]
```

## Motivation

Javascript's `Object` type is used extremely ubiquitously, and in practice serves the purpose of both the C-style `struct`, as well as the python-style `dict`. Unfortunately, common operations on this ubiquitous type cannot effectively be implemented on `Object.prototype`. Objects can have a large number of dynamically assigned properties, and hence convenient patterns such as `myObject.hasOwnProperty(...)` cannot be relied on, as the term "hasOwnProperty" may have been set as an "own" property, masking the reference to `Object.prototype.hasOwnProperty`. (We unfortunately see `{}.hasOwnProperty.call(myObject, ...)` used instead!)

I believe Javascript would benefit greatly from utility methods available on `Object.prototype`. Some example utility methods (*shown **only** to illustrate the potential of "prototype-only-dereferencing"; these example utilities are **not** part of this proposal*):

```
const myObject = { a: 1, b: 2 };
console.log(myObject..map(v => v + 1)); // { a: 2, b: 3 }
```

```
const myObject = { a: { b: { c: 1 } } };
console.log(myObject..at('z')); // undefined
console.log(myObject..at('a')); // { b: { c: 1 } }
console.log(myObject..at([ 'a', 'b', 'c' ])); // 1
```

By allowing developer-friendly access to object prototype properties without fear of masking, we enable a future where one of Javascript's most well-used types is supported by a host of utilities defined on the prototype.