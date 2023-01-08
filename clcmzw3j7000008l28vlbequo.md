# 10 JavaScript Operators You Don't Know Exist

I have a quick quiz for you!! Can you try it?

```javascript
console.log(5 & 2); // your answer??
console.log(5 | 2); //...
console.log(new.target); 
console.log(import.meta) 

const a = { duration: 50 };
a.duration ??= 10;
console.log(a.duration);
// Output: ...

a.speed ??= 25;
console.log(a.speed);
// Output: ...
```

If it is your first time seeing this stuff in JS don't worry I am about to go throw them all and more...

## `new.target`

The `new.target` will let you detect whether a **function or constructor** was called using the `new` operator.

In the class constructor, it refers to the class that the `new` was called upon.

In ordinary functions, if the function was called by new it will refer to the function itself. if the function is called without `new`, `new.target` is `undefined`

```javascript
// Use `new.target` with function constructor 
function CallMe() {
    if (!new.target) throw new Error("Calling function constructor without new is invalid");
    // ...
}

try {
    CallMe()
} catch (e) {
    console.log(e);
    // Output: TypeError: Calling function constructor without new is invalid
}
```

```javascript
// Class constructor 
class Car {
    constructor() {
        console.log(new.target.name);
    }
}

class Toyota extends Car {
    constructor() {
        super();
    }
}

const car = new Car(); // Logs: Car
const toyota = new Toyota(); // Logs: Toyota
```

## Nullish coalescing assignment (`??=`)

The **nullish coalescing assignment (**`x ??= y`) operator only assigns if `x` is [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) (`null` or `undefined`).

```javascript
let person = { name: 'ali' };

person.name ??= 'no-name';
person.age ??= 10;

console.log(person);
// Output: {name: 'ali', age: 10}
```

## `import.meta`

It contains information about the module such as the module's URL.

```xml
<script type="module">
    import "./app.js?blogName=DeepCode&g=4";
</script>
```

```javascript
// app.js 
console.log(import.meta);
const asUrl = new URL(import.meta.url)
console.log(asURL.searchParams.get('blogName')); // DeepCode
console.log(asURL.searchParams.get('g')); // 4
```

## `import()`

it is a function-like expression that allows loading a JS module **asynchronously** and **dynamically** into a potentially non-module environment. it is also called **<mark>Dynamic imports.</mark>**

```javascript
// math.js
export function add(a, b) {
    return a + b;
}
```

```javascript
// app.js 
// will return a Promise!
import('./math.js').then(math => {
    const res = math.add(-1, 1);
    // ...
})
```

# Bitwise Operators

Also, JS is a high-level programming language it still lets you operate on the **bit** level! if you are coming from low-level languages like C / C++ you will feel you are at home!

The bitwise operator works on the bit level which means it doesn't look at the actual value of the number but all that it can see is a series of **0s and 1s (the binary representation).**

Note that all numbers in JavaScript are **64-bit (double-precision)** floating-point numbers. To make it simple I will not write the whole bit series instead I will just focus on the first 8 bits.

## Left Shift (`<<`)

If we took the number `2 (0000 0010)` and did a **left shift by 1** it will move all bits to the **left by 1** and we will get `4 (0000 0100)`. Excess bits shifted off to the left will be discarded. Zero bits will be added from the right.

Note that JS will not output the bits in the console instead it will convert them into numbers.

```javascript
const a = 2; // 0000 0010
const b = 1;

console.log(a << b); // 0000 0100
// Output: 4

// 2:      0000 0010
// 2 << 1: 0000 0100
```

## Right Shift (`<<`)

The **right shift** operator is the same as the **left shift** but to the right.

```javascript
const a = 5; // 0000 0101
const b = 2;

console.log(a >> b); // 00000001
// Output: 1

// 5:        0000 0101
// 5 >> 2 :  0000 0001 (1)
```

# Unsigned right shift (`>>>`)

This operator will still shift the bit to the right but this time the **sign bit will become 0** so the result will always be non-negative.

```javascript
const a = 5;     //  00000000000000000000000000000101
const b = 2;     //  00000000000000000000000000000010
const c = -5;    //  11111111111111111111111111111011

console.log(a >>> b); //  00000000000000000000000000000001
// Output: 1

console.log(c >>> b); //  00111111111111111111111111111110
// Output: 1073741822
```

## Bitwise AND (`&`)

The Bitwise AND (&) will compare every bit from the first number to the second number at the same position (1st with 1st, 2nd with 2nd, ....) and will return `1` only if the two bits from the 1st and 2nd number are `1`

```javascript
// 1 & 1 = 1
// 1 & 0 = 0
// 0 & 0 = 0
// 0 & 1 = 0

// a:     0101
// b:     1011
// a & b: 0001
```

```javascript
const a = 5; // 00000101
const b = 3; // 00000011

console.log(a & b); // 00000001
// Output: 1
```

## Bitwise NOT (`~`)

The **bitwise NOT (**`~`**)** operator will invert the bits of the number.

```javascript
// ~1 = 0
// ~0 = 1

// a:  0101
// ~a: 1010
```

```javascript
const a = 5; // 00000101
const b = 3; // 00000011

console.log(~a); // 11111010 (-6)
console.log(~b); // 11111100 (-4)
```

## Bitwise OR (`|`)

The **bitwise OR (**`|`**)** operator will return 1 if both or either corresponding bits is 1.

```javascript
// 1 | 1 = 1
// 1 | 0 = 1
// 0 | 0 = 0
// 0 | 1 = 1

// a(5):      0101
// b(11):     1011
// a | b(15): 1111
```

```javascript
const a = 5; // 00000101
const b = 3; // 00000011

console.log(a | b); // 00000111
// Output: 7
```

## Bitwise XOR (`^`)

The **bitwise XOR (**`^`) operator returns a `1` in each bit position for which the corresponding bits of either but not both operands are `1`s.

```javascript
// 1 ^ 1 = 0
// 1 ^ 0 = 1
// 0 ^ 0 = 0
// 0 ^ 1 = 1

// a(5):       0101
// b(11):      1011
// a ^ b (14): 1110
```

```javascript
const a = 5; // 00000101
const b = 3; // 00000011

console.log(a ^ b); // 00000110
// Output: 6
```