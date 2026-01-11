# 📗 JavaScript Advanced Concepts - Documentation

Deep dive into closures, prototypes, async programming, and ES6+ features.

---

## Table of Contents
1. [Scope & Closures](#1-scope--closures)
2. [The `this` Keyword](#2-the-this-keyword)
3. [Prototypes & Inheritance](#3-prototypes--inheritance)
4. [Promises & Async/Await](#4-promises--asyncawait)
5. [Event Loop & Execution Order](#5-event-loop--execution-order)
6. [Higher-Order Functions](#6-higher-order-functions)
7. [Generators & Iterators](#7-generators--iterators)
8. [Proxy & Reflect](#8-proxy--reflect)

---

## 1. Scope & Closures

### Scope Types

| Scope Type | Access | Created By |
|------------|--------|------------|
| Global | Everywhere | Outside any function |
| Function | Inside function | `function` keyword |
| Block | Inside `{}` | `let`/`const` only |

### Scope Chain Example
```javascript
let global = "I'm global";

function outer() {
    let outerVar = "I'm from outer";
    
    function inner() {
        let innerVar = "I'm from inner";
        console.log(innerVar);  // Own scope
        console.log(outerVar);  // Parent scope
        console.log(global);    // Global scope
    }
    inner();
}
```
**Key Insight**: Inner functions can access variables from all outer scopes.

---

### Closures Concept
**Definition**: A closure is a function that remembers and can access its outer scope variables even after the outer function has returned.

```javascript
function createCounter() {
    let count = 0;  // Private variable
    
    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3

const counter2 = createCounter(); // New closure
console.log(counter2()); // 1
```
**Use Cases**: Data privacy, factory functions, memoization.

---

### Q1: Calculator Factory
**Description**: Returns object with chainable methods using closures for state.

```javascript
function calculator(initialValue = 0) {
    let value = initialValue;
    
    return {
        add(n) { value += n; return this; },
        subtract(n) { value -= n; return this; },
        multiply(n) { value *= n; return this; },
        divide(n) { value /= n; return this; },
        getValue() { return value; }
    };
}

const calc = calculator(10);
console.log(calc.add(5).multiply(2).subtract(10).getValue()); // 20
```
**Pattern**: Method chaining with `return this`.

---

### Q2: Classic Closure Interview Problem
**Description**: Demonstrates the var vs let scoping issue in loops.

```javascript
// Problem: var is function-scoped
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log("var:", i), 100);
}
// Output: 3, 3, 3

// Solution 1: Use let (block-scoped)
for (let j = 0; j < 3; j++) {
    setTimeout(() => console.log("let:", j), 200);
}
// Output: 0, 1, 2

// Solution 2: Use IIFE
for (var k = 0; k < 3; k++) {
    ((k) => {
        setTimeout(() => console.log("IIFE:", k), 300);
    })(k);
}
// Output: 0, 1, 2
```

---

### Q3: Memoization Function
**Description**: Caches function results to avoid redundant computations.

```javascript
function memoize(fn) {
    const cache = {};
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache[key] !== undefined) {
            console.log('From cache');
            return cache[key];
        }
        
        console.log('Computing');
        const result = fn.apply(this, args);
        cache[key] = result;
        return result;
    };
}

const expensiveAdd = memoize((a, b) => a + b);
console.log(expensiveAdd(1, 2)); // Computing -> 3
console.log(expensiveAdd(1, 2)); // From cache -> 3
```
| Complexity | Value |
|------------|-------|
| Time | O(1) for cached calls |
| Space | O(n) cache entries |

---

## 2. The `this` Keyword

### `this` Binding Rules (Priority Order)

| Rule | How `this` is determined | Example |
|------|-------------------------|---------|
| 1. `new` | New object | `new Foo()` |
| 2. Explicit | Specified object | `fn.call(obj)` |
| 3. Implicit | Object before dot | `obj.fn()` |
| 4. Default | `globalThis` or `undefined` (strict) | `fn()` |

### call, apply, bind

```javascript
function greet(greeting, punctuation) {
    console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: "Alice" };

// call - invoke immediately with args
greet.call(person, "Hello", "!");  // "Hello, Alice!"

// apply - invoke immediately with args array
greet.apply(person, ["Hi", "?"]);  // "Hi, Alice?"

// bind - return new function with bound this
const boundGreet = greet.bind(person);
boundGreet("Hey", ".");  // "Hey, Alice."
```

---

### Q4: Custom call, apply, bind
**Description**: Implements these methods from scratch.

```javascript
// Custom call
Function.prototype.myCall = function(context, ...args) {
    context = context || globalThis;
    const sym = Symbol();
    context[sym] = this;
    const result = context[sym](...args);
    delete context[sym];
    return result;
};

// Custom apply
Function.prototype.myApply = function(context, args = []) {
    return this.myCall(context, ...args);
};

// Custom bind
Function.prototype.myBind = function(context, ...args) {
    const fn = this;
    return function(...newArgs) {
        return fn.myCall(context, ...args, ...newArgs);
    };
};
```

---

## 3. Prototypes & Inheritance

### Prototype Chain
```
Object Instance → Constructor.prototype → Object.prototype → null
```

```javascript
const arr = [1, 2, 3];
arr.__proto__ === Array.prototype;         // true
Array.prototype.__proto__ === Object.prototype; // true
Object.prototype.__proto__;                 // null (end of chain)
```

### ES6 Classes
```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        return `${this.name} makes a sound`;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);  // Call parent constructor
        this.breed = breed;
    }
    speak() {
        return `${this.name} barks!`;
    }
}

const dog = new Dog("Rex", "German Shepherd");
console.log(dog.speak());        // "Rex barks!"
console.log(dog instanceof Dog);    // true
console.log(dog instanceof Animal); // true
```

---

### Q5: Classical Inheritance (Pre-ES6)
**Description**: Implements inheritance using constructor functions.

```javascript
// Parent
function Vehicle(make, model) {
    this.make = make;
    this.model = model;
}
Vehicle.prototype.getInfo = function() {
    return `${this.make} ${this.model}`;
};

// Child
function Car(make, model, doors) {
    Vehicle.call(this, make, model);  // Call parent constructor
    this.doors = doors;
}

// Set up inheritance
Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;

// Add child method
Car.prototype.honk = function() { return "Beep beep!"; };

const myCar = new Car("Toyota", "Camry", 4);
console.log(myCar.getInfo()); // "Toyota Camry"
console.log(myCar instanceof Vehicle); // true
```

---

## 4. Promises & Async/Await

### Promise States
| State | Meaning | Transitions To |
|-------|---------|----------------|
| Pending | Initial state | Fulfilled or Rejected |
| Fulfilled | Completed successfully | - |
| Rejected | Failed with error | - |

### Creating & Consuming Promises
```javascript
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        success ? resolve("Success!") : reject(new Error("Failed!"));
    }, 1000);
});

myPromise
    .then(result => console.log(result))
    .catch(error => console.error(error))
    .finally(() => console.log("Done"));
```

### Promise Static Methods

| Method | Behavior | Use Case |
|--------|----------|----------|
| `Promise.all` | Fails fast, returns array | Wait for all (fail on first error) |
| `Promise.allSettled` | Never fails, returns results | Wait for all regardless of success |
| `Promise.race` | Returns first to settle | Timeout patterns |
| `Promise.any` | Returns first fulfilled | First success wins |

---

### Async/Await Syntax
```javascript
async function getUserWithPosts(id) {
    try {
        const user = await fetchUser(id);
        const posts = await fetchPosts(user.id);
        return { user, posts };
    } catch (error) {
        console.error("Error:", error);
    }
}
```

---

### Q6: Custom Promise.all
**Description**: Implements Promise.all from scratch.

```javascript
function myPromiseAll(promises) {
    return new Promise((resolve, reject) => {
        const results = [];
        let completed = 0;
        
        if (promises.length === 0) {
            resolve(results);
            return;
        }
        
        promises.forEach((promise, index) => {
            Promise.resolve(promise)
                .then(value => {
                    results[index] = value;
                    completed++;
                    if (completed === promises.length) resolve(results);
                })
                .catch(reject);
        });
    });
}
```

---

### Q7: Retry Mechanism
**Description**: Retries a function up to N times with exponential backoff.

```javascript
async function retry(fn, maxRetries = 3, delay = 1000) {
    let lastError;
    
    for (let i = 0; i < maxRetries; i++) {
        try {
            return await fn();
        } catch (error) {
            lastError = error;
            console.log(`Attempt ${i + 1} failed. Retrying...`);
            if (i < maxRetries - 1) {
                await new Promise(r => setTimeout(r, delay));
            }
        }
    }
    throw lastError;
}
```

---

## 5. Event Loop & Execution Order

### Task Queue Priority
1. **Synchronous code** (call stack)
2. **Microtasks** (Promises, `queueMicrotask`)
3. **Macrotasks** (`setTimeout`, `setInterval`, I/O)

```javascript
console.log("1: Start");
setTimeout(() => console.log("2: Timeout 0"), 0);
Promise.resolve().then(() => console.log("3: Promise 1"));
Promise.resolve().then(() => {
    console.log("4: Promise 2");
    Promise.resolve().then(() => console.log("5: Nested Promise"));
});
console.log("6: End");

// Output order: 1, 6, 3, 4, 5, 2
```

---

### Q8: Async Execution Order Prediction
**Description**: Classic interview question on event loop behavior.

```javascript
async function async1() {
    console.log("async1 start");  // 2
    await async2();
    console.log("async1 end");    // 6
}

async function async2() {
    console.log("async2");        // 3
}

console.log("script start");      // 1
setTimeout(() => console.log("setTimeout"), 0);  // 8
async1();
new Promise(resolve => {
    console.log("promise1");      // 4
    resolve();
}).then(() => console.log("promise2"));  // 7
console.log("script end");        // 5

// Output: script start, async1 start, async2, promise1, script end, async1 end, promise2, setTimeout
```

---

## 6. Higher-Order Functions

### Function Composition
```javascript
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);

const add10 = x => x + 10;
const mult2 = x => x * 2;

compose(add10, mult2)(5);  // (5 * 2) + 10 = 20
pipe(add10, mult2)(5);     // (5 + 10) * 2 = 30
```

---

### Q9: Curry Function
**Description**: Transforms a function to accept arguments one at a time.

```javascript
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        }
        return function(...moreArgs) {
            return curried.apply(this, args.concat(moreArgs));
        };
    };
}

function add(a, b, c) { return a + b + c; }
const curriedAdd = curry(add);

console.log(curriedAdd(1)(2)(3)); // 6
console.log(curriedAdd(1, 2)(3)); // 6
console.log(curriedAdd(1)(2, 3)); // 6
```

---

### Q10: Debounce and Throttle
**Description**: Rate-limiting functions for performance optimization.

| Pattern | Behavior | Use Case |
|---------|----------|----------|
| Debounce | Wait until user stops | Search input, resize |
| Throttle | Execute at most once per interval | Scroll, mousemove |

```javascript
// Debounce: Wait until user stops triggering
function debounce(fn, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => fn.apply(this, args), delay);
    };
}

// Throttle: Execute at most once per interval
function throttle(fn, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            fn.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```

---

## 7. Generators & Iterators

### Generator Functions
```javascript
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = numberGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

---

## 8. Proxy & Reflect

### Proxy for Object Interception
```javascript
const handler = {
    get(target, prop) {
        console.log(`Getting ${prop}`);
        return target[prop];
    },
    set(target, prop, value) {
        console.log(`Setting ${prop} to ${value}`);
        target[prop] = value;
        return true;
    }
};

const proxy = new Proxy({}, handler);
proxy.name = "John";  // Setting name to John
console.log(proxy.name);  // Getting name → "John"
```

---

## 📊 Quick Reference

### `this` Binding Summary
| Call Type | `this` Value |
|-----------|-------------|
| `obj.fn()` | `obj` |
| `fn()` | `globalThis` / `undefined` (strict) |
| `fn.call(ctx)` | `ctx` |
| `new Fn()` | New instance |
| Arrow | Inherited from parent |

### Promise Methods Comparison
| Method | Resolves When | Rejects When |
|--------|---------------|--------------|
| `all` | All fulfill | Any rejects |
| `allSettled` | All settle | Never |
| `race` | First settles | First rejects |
| `any` | First fulfills | All reject |

---

*📚 Previous: [JavaScript Fundamentals](01_JavaScript_Fundamentals.md) | Next: [DSA - Data Structures](03_DSA_Data_Structures.md)*
