# Flow Snippets

## Object Types

```js
interface FooBarBaz {
  foo: number;
  bar: boolean;
  baz: string;
}

const obj1: {
  foo: number,
  bar: boolean,
  baz: string
} = {
  foo: 1,
  bar: true,
  baz: "three"
};

const obj2: FooBarBaz = {
  foo: 1,
  bar: true,
  baz: "three"
};

function makeFooBarBaz(): FooBarBaz {
  return {
    foo: 1,
    bar: true,
    baz: "three"
  };
}

const makeFooBarArrow = (): FooBarBaz => {
  return {
    foo: 1,
    bar: true,
    baz: "three"
  };
};
```

# Types

Types can describe an object type or a function signature. 

**Object Type**

```js
type Person = {
  age: number,
  name: string
};

const student: Person = {
  age: 27,
  name: "Dave"
};

// note the object return type
function getStudentById(id: number): Person {
  return {
    age: 23,
    name: "John"
  };
}
````

**Function Signature Type**

```js
type Signature = (age: number, name: string) => string;

function sayHello(callbackFn: Signature) {
    
}
```

## Function Types

```js
type output = {
  valid: boolean,
  int?: number
};

function toIntAlt(str: string): output {
  const int = parseInt(str);
  if (isNaN(int)) {
    return { valid: false };
  } else {
    return { valid: true, int };
  }
}
```

## Function Signature

### Example 1

```js
type SampleType = (str: string) => { valid: boolean, int?: number };

const makeSampleType: SampleType = str => {
  return {
    valid: false,
  };
};
```

### Example 2

```js
type MethodType = (str: string, bool: boolean, nums: number[]) => void;

const method: MethodType = (x, y, b) => {
  console.log(x, y, b);
};

method("a", true, [1, 2, 3]);
```

## Function Type

### Example 1

```js
type ExtractedSignature = (age: number, name: string) => string;

const full: (age: number, name: string) => string = function(): string {
  return "";
};

const terse: (age: number, name: string) => string = (): string => "";

const terser: ExtractedSignature = (): string => "";

const tersest: ExtractedSignature = () => "";

// usage
tersest(5, "John"); //=> // ✓ OK 
tersest(5); //=> ✗ Error:(213, 1) Cannot call `tersest` because function [1] requires another argument.
```

### Example 2

As opposed to `Example 1`, review this similar looking terse function and notice that it means something completely different.

```js
type ExtractedSignature = (age: number, name: string) => string;


type MethodType = (str: string, bool: boolean, nums: number[]) => void;

const method: MethodType = (x, y, b) => {
  console.log(x, y, b);
};

const method2 = (): MethodType => function(a, b) {};
```

What does `TheType` instruct us to do in order to implement it? It tells us that it is a function signature that takes two arguments, a string and a boolean and returns void.

* `method1` implements the type as it's function signature
* `method2` gives the following error
    * Error:(224, 21) Cannot expect `TheType` as the return type of function because `TheType` [1] is incompatible with implicitly-returned undefined.
* `method3` gives the following error
  * Error:(226, 42) Cannot return function because number [1] is incompatible with undefined [2] in the return value.
* `method4` passes because it implements `TheType`'s signature - that is, it returns void
* `method5` passes suprsingly even though the returned function doesn't take any arguements
  * upon closer inspection,  Flow will warn us of this mistake when calling `method5`'s returned function
  * Error:(242, 1) Cannot call `callableReturnedFromMethodFive` because function [1] requires another argument.

Take note that methods 2, 3, and 4 implement `TheType` as the signature of their return value while method 1 implements it as its function signature.

In other words:

* `method1` - says to take a string and a bool and return void
* `method2...4` - says take nothing, but return a function that takes a string and a bool that itself returns void

```js
type TheType = (str: string, bool: boolean) => void;

const method1: TheType = (x, y) => {};       // ✓ OK implements the function signature type
const method2 = (): TheType => {};           // ✗
const method3 = (): TheType => (a, b) => 5;  // ✗
const method4 = (): TheType => (a, b) => {}; // ✓ OK
const method5 = (): TheType => () => {};     // ✓ OK

const callableReturnedFromMethodFive = method5(); // ✓ OK
callableReturnedFromMethodFive(); // ✗
```

Let's take a deeper look at `method5`. Below is `method5` (now called `method5_0`) rewritten in different but still syntacticlly legal ways (nonetheless confusing).

```js
type TheType = (str: string, bool: boolean) => void;

const method5_0 = (): TheType => () => {};
const method5_1 = (): TheType => function() {};

const method5_2 = function(): TheType {
  return function() {};
};

function method5_3(): TheType {
  return function() {};
}

// expectedly, they all throw errors
method5_0()(); // ✗
method5_1()(); // ✗
method5_2()(); // ✗
method5_3()(); // ✗
```

All of this is to say, that we cannot replicate the desired behaviour of the original `method1` implementation using non-arrow function style. Take a look below for clarity.

* `method1_0` and `method1_1` are equivalent and use `TheType` as their function signature
* `method1_2` is not equivalent to either `method1_0` or `method1_1`
  * `method1_2` attempts to use `TheType` but the only syntactically legal place is as the return type
* `method1_3` is equivalent to `method1_0` and `method1_1` but must have the signature declared in-place and not as a reference

```js
type TheType = (str: string, bool: boolean) => void;

const method1_0: TheType = (x, y) => {};
const method1_1: TheType = function (x, y) {
    return;
};  

function method1_2(x, y): TheType {
  return function(a, b) {

  };
}

function method1_3(str: string, bool: boolean): void {
  return;
}
```

## Types of Types

```js
type Props = {
  bar: number
}

type FunctionSignatureType = (x: int, y: int) => int;
```

## Wierd

All these are valid.

```js
// @flow

function filter1<T> (
  list: Array<T>,
  callback: (item: T) => boolean
): Array<T> {
  return list.filter(callback);
}

// previous function re-organized
function filter1_1<T>(list: Array<T>, callback: (item: T) => boolean): Array<T> {
  return list.filter(callback);
}

const filter2 = <T>(list: Array<T>, callback: (item: T) => boolean): Array<T> => list.filter(callback);

const filter3 = function <T>(list: Array<T>, callback: (item: T) => boolean): Array<T> {
  return list.filter(callback)
};

type ExtractedSignature = <T>(list: Array<T>, callback: (item: T) => boolean) => Array<T>;

const filter4 : ExtractedSignature = <T>(list: Array<T>, callback: (item: T) => boolean): Array<T> => list.filter(callback);

// previous function reorganized
const filter4_1: ExtractedSignature = <T>(
  list: Array<T>,
  callback: (item: T) => boolean
): Array<T> => list.filter(callback);
```
