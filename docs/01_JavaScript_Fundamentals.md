# 📘 JavaScript Fundamentals - Documentation

A comprehensive guide covering JavaScript basics to intermediate concepts with Q&A format.

---

## Table of Contents
1. [Variables & Data Types](#1-variables--data-types)
2. [Operators & Type Coercion](#2-operators--type-coercion)
3. [Control Flow](#3-control-flow)
4. [Loops](#4-loops)
5. [Functions](#5-functions)
6. [Arrays](#6-arrays)
7. [Objects](#7-objects)
8. [Strings](#8-strings)
9. [Error Handling](#9-error-handling)

---

## 1. Variables & Data Types

### Concept Overview
JavaScript has three ways to declare variables:
- **`var`**: Function-scoped, hoisted, can be redeclared
- **`let`**: Block-scoped, not hoisted (TDZ), cannot be redeclared
- **`const`**: Block-scoped, not hoisted (TDZ), cannot be reassigned

### Data Types
| Type | Category | Example |
|------|----------|---------|
| `string` | Primitive | `"Hello"` |
| `number` | Primitive | `42`, `3.14` |
| `boolean` | Primitive | `true`, `false` |
| `null` | Primitive | `null` |
| `undefined` | Primitive | `undefined` |
| `symbol` | Primitive | `Symbol('id')` |
| `bigint` | Primitive | `9007199254740991n` |
| `object` | Non-primitive | `{}`, `[]`, functions |

### Programs

#### Variable Declarations Demo
```javascript
var name = "John";      // Function scoped
let age = 25;           // Block scoped
const PI = 3.14159;     // Cannot be reassigned
```
**Purpose**: Demonstrates the three variable declaration methods and their behaviors.

---

### Q1: Difference Between `null` and `undefined`
**Description**: Explains the distinction between intentional absence of value vs unassigned value.

| Aspect | `undefined` | `null` |
|--------|-------------|--------|
| Meaning | Variable declared but not assigned | Intentional absence of value |
| Assignment | Automatic | Explicit |
| typeof | `"undefined"` | `"object"` (JS quirk) |

```javascript
let a;           // undefined - not assigned
let b = null;    // null - intentionally empty

console.log(null == undefined);  // true (loose equality)
console.log(null === undefined); // false (strict equality)
```

---

### Q2: Variable Hoisting with `var`
**Description**: Demonstrates how `var` declarations are hoisted to the top of their scope.

```javascript
console.log(x); // Output: undefined
var x = 5;

// Interpreted as:
var x;
console.log(x); // undefined
x = 5;
```
**Key Insight**: `var` is hoisted but only the declaration, not the initialization.

---

### Q3: Temporal Dead Zone (TDZ) with `let`
**Description**: Shows how `let` variables cannot be accessed before declaration.

```javascript
// console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
console.log(y); // 10 - works after declaration
```
**Key Insight**: `let` and `const` exist in a TDZ from block start until declaration.

---

## 2. Operators & Type Coercion

### Operators Reference

| Category | Operators | Example |
|----------|-----------|---------|
| Arithmetic | `+`, `-`, `*`, `/`, `%`, `**` | `10 % 3 = 1` |
| Comparison (Loose) | `==`, `!=` | `5 == "5"` → `true` |
| Comparison (Strict) | `===`, `!==` | `5 === "5"` → `false` |
| Logical | `&&`, `\|\|`, `!` | `true && false` → `false` |

---

### Q4: Truthy and Falsy Values
**Description**: Lists all falsy values in JavaScript and demonstrates Boolean coercion.

**Falsy Values** (evaluate to `false`):
- `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`

**Everything else is truthy**, including:
- Empty arrays `[]`
- Empty objects `{}`
- String `"0"` or `"false"`

```javascript
console.log(Boolean(0));     // false
console.log(Boolean(""));    // false
console.log(Boolean([]));    // true (empty array is truthy!)
console.log(Boolean({}));    // true (empty object is truthy!)
```

---

### Q5: Type Coercion Quirks
**Description**: Demonstrates JavaScript's automatic type conversion in operations.

```javascript
console.log("5" + 3);      // "53" (string concatenation)
console.log("5" - 3);      // 2 (numeric subtraction)
console.log("5" * "2");    // 10 (numeric multiplication)
console.log(true + true);  // 2 (true = 1)
console.log([] + []);      // "" (empty string)
console.log([] + {});      // "[object Object]"
```
**Key Insight**: `+` with strings triggers concatenation; other operators trigger numeric coercion.

---

## 3. Control Flow

### Programs

#### If-Else Statement
```javascript
let score = 85;

if (score >= 90) {
    console.log("A Grade");
} else if (score >= 80) {
    console.log("B Grade");  // Output: "B Grade"
} else if (score >= 70) {
    console.log("C Grade");
} else {
    console.log("Need Improvement");
}
```
**Purpose**: Basic conditional branching based on conditions.

---

#### Switch Statement
```javascript
let day = 3;

switch (day) {
    case 1: console.log("Monday"); break;
    case 2: console.log("Tuesday"); break;
    case 3: console.log("Wednesday"); break;  // Output
    default: console.log("Other day");
}
```
**Purpose**: Multi-way branching for discrete value matching.

---

#### Ternary & Nullish Coalescing
```javascript
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";  // "Adult"

let user = null;
let defaultUser = user ?? "Guest";  // "Guest"
```
**Purpose**: Inline conditionals and null-safe defaults.

---

### Q6: Difference Between `||` and `??`
**Description**: Explains short-circuit evaluation with logical OR vs nullish coalescing.

| Operator | Checks For | Returns |
|----------|------------|---------|
| `\|\|` | All falsy values | First truthy value |
| `??` | Only `null`/`undefined` | First defined value |

```javascript
let count = 0;
console.log(count || 10);  // 10 (0 is falsy)
console.log(count ?? 10);  // 0 (0 is not null/undefined)

let empty = "";
console.log(empty || "default"); // "default"
console.log(empty ?? "default"); // "" (empty string is defined)
```

---

## 4. Loops

### Programs

#### For, While, Do-While Loops
```javascript
// For Loop
for (let i = 0; i < 5; i++) {
    console.log(i); // 0, 1, 2, 3, 4
}

// While Loop
let j = 0;
while (j < 3) {
    console.log(j++); // 0, 1, 2
}

// Do-While Loop (runs at least once)
let k = 0;
do {
    console.log(k++); // 0, 1, 2
} while (k < 3);
```

---

#### For...of and For...in Loops
```javascript
// For...of (iterables - arrays, strings)
for (let fruit of ["apple", "banana"]) {
    console.log(fruit);
}

// For...in (object keys)
for (let key in { name: "John", age: 30 }) {
    console.log(key); // "name", "age"
}
```

---

### Q7: Difference Between `for...in` and `for...of`
**Description**: Clarifies when to use each iterator.

| Aspect | `for...in` | `for...of` |
|--------|-----------|------------|
| Iterates | Keys/indices | Values |
| Use with | Objects | Arrays, strings, iterables |
| Returns | String indices | Actual values |

```javascript
let arr = ["a", "b", "c"];

for (let index in arr) console.log(index); // "0", "1", "2" (strings!)
for (let value of arr) console.log(value); // "a", "b", "c"
```

---

## 5. Functions

### Function Types

| Type | Hoisted | `this` Binding | Use Case |
|------|---------|----------------|----------|
| Declaration | Yes | Dynamic | General functions |
| Expression | No | Dynamic | Callbacks, closures |
| Arrow | No | Lexical (inherits) | Short callbacks, no `this` |

```javascript
// Declaration (hoisted)
function greet(name) { return `Hello, ${name}!`; }

// Expression (not hoisted)
const greet2 = function(name) { return `Hello, ${name}!`; };

// Arrow (ES6)
const greet3 = (name) => `Hello, ${name}!`;
```

---

### Default & Rest Parameters
```javascript
// Default Parameters
function multiply(a, b = 1) { return a * b; }
console.log(multiply(5));    // 5
console.log(multiply(5, 3)); // 15

// Rest Parameters
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

---

### Q8: Arrow Functions vs Regular Functions
**Description**: Key differences between arrow and regular functions.

| Feature | Regular Function | Arrow Function |
|---------|-----------------|----------------|
| `this` | Dynamic binding | Inherits from parent |
| `arguments` | Has access | No access |
| Constructor | Can use `new` | Cannot use `new` |
| Hoisting | Declarations hoisted | Not hoisted |

---

### Q9: isPrime Function
**Description**: Checks if a number is prime using optimized trial division.

```javascript
function isPrime(n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 === 0 || n % 3 === 0) return false;
    
    for (let i = 5; i * i <= n; i += 6) {
        if (n % i === 0 || n % (i + 2) === 0) return false;
    }
    return true;
}
```
| Complexity | Value |
|------------|-------|
| Time | O(√n) |
| Space | O(1) |

---

## 6. Arrays

### Array Creation Methods
```javascript
let arr1 = [1, 2, 3];                    // Literal
let arr2 = new Array(3);                  // [empty × 3]
let arr3 = Array.from("hello");           // ['h','e','l','l','o']
let arr4 = Array(5).fill(0);              // [0, 0, 0, 0, 0]
```

### Array Methods Reference

| Method | Returns | Mutates | Purpose |
|--------|---------|---------|---------|
| `forEach` | `undefined` | No | Iterate |
| `map` | New array | No | Transform |
| `filter` | New array | No | Select |
| `reduce` | Single value | No | Accumulate |
| `find` | First match | No | Search |
| `some/every` | Boolean | No | Test condition |
| `push/pop` | Length/removed | Yes | End operations |
| `shift/unshift` | Removed/length | Yes | Start operations |

---

### Q10: Custom Array Method Implementations
**Description**: Implements `map`, `filter`, and `reduce` from scratch.

```javascript
// Custom map
Array.prototype.myMap = function(callback) {
    const result = [];
    for (let i = 0; i < this.length; i++) {
        result.push(callback(this[i], i, this));
    }
    return result;
};

// Custom filter
Array.prototype.myFilter = function(callback) {
    const result = [];
    for (let i = 0; i < this.length; i++) {
        if (callback(this[i], i, this)) result.push(this[i]);
    }
    return result;
};

// Custom reduce
Array.prototype.myReduce = function(callback, initialValue) {
    let acc = initialValue !== undefined ? initialValue : this[0];
    let startIndex = initialValue !== undefined ? 0 : 1;
    
    for (let i = startIndex; i < this.length; i++) {
        acc = callback(acc, this[i], i, this);
    }
    return acc;
};
```

---

### Q11: Flatten Nested Array
**Description**: Three approaches to flatten deeply nested arrays.

```javascript
// Method 1: Built-in flat()
[1, [2, [3, [4]]]].flat(Infinity); // [1, 2, 3, 4]

// Method 2: Recursive
function flatten(arr) {
    return arr.reduce((flat, item) => 
        flat.concat(Array.isArray(item) ? flatten(item) : item), []);
}

// Method 3: Iterative with stack
function flattenIterative(arr) {
    const stack = [...arr], result = [];
    while (stack.length) {
        const item = stack.pop();
        Array.isArray(item) ? stack.push(...item) : result.unshift(item);
    }
    return result;
}
```

---

## 7. Objects

### Object Methods Reference

| Method | Returns | Purpose |
|--------|---------|---------|
| `Object.keys()` | Array of keys | Get property names |
| `Object.values()` | Array of values | Get property values |
| `Object.entries()` | Array of [key, value] | Get key-value pairs |
| `Object.assign()` | Merged object | Shallow copy/merge |

---

### Q12: Deep Clone Object
**Description**: Recursively clones objects, handling nested objects, arrays, and Dates.

```javascript
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') return obj;
    if (obj instanceof Date) return new Date(obj.getTime());
    if (Array.isArray(obj)) return obj.map(item => deepClone(item));
    
    const cloned = {};
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            cloned[key] = deepClone(obj[key]);
        }
    }
    return cloned;
}
```
**Note**: For modern JS, use `structuredClone()` for built-in deep cloning.

---

## 8. Strings

### Common String Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `split()` | String → Array | `"a,b".split(",")` → `["a","b"]` |
| `join()` | Array → String | `["a","b"].join(",")` → `"a,b"` |
| `slice()` | Extract substring | `"hello".slice(1,3)` → `"el"` |
| `includes()` | Check contains | `"hello".includes("ell")` → `true` |
| `replace()` | Replace first | `"aa".replace("a","b")` → `"ba"` |
| `replaceAll()` | Replace all | `"aa".replaceAll("a","b")` → `"bb"` |

---

## 9. Error Handling

### Try-Catch-Finally Pattern
```javascript
try {
    // Code that might throw
    const data = JSON.parse(invalidJson);
} catch (error) {
    // Handle error
    console.error("Parse failed:", error.message);
} finally {
    // Always runs
    cleanup();
}
```

### Custom Error Types
```javascript
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

throw new ValidationError("Invalid input");
```

---

## 📊 Quick Reference

### Falsy Values Checklist
- [ ] `false`
- [ ] `0`, `-0`, `0n`
- [ ] `""` (empty string)
- [ ] `null`
- [ ] `undefined`
- [ ] `NaN`

### Variable Declaration Comparison
| Feature | `var` | `let` | `const` |
|---------|-------|-------|---------|
| Scope | Function | Block | Block |
| Hoisting | Yes | TDZ | TDZ |
| Redeclare | Yes | No | No |
| Reassign | Yes | Yes | No |

---

*📚 Next: [JavaScript Advanced](02_JavaScript_Advanced.md)*
