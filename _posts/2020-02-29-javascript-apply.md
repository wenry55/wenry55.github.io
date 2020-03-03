---
layout: default
title: javascript apply syntax
categories: [programming, javascript]
tags: [javascript, syntax, apply]
---

# apply
##### invoke a function as a method of an object

>이 함수는 함수형 오브젝트를 파라메터로 주어진 오브젝트의 메서드로 적용한다는 뜻으로 이해하면 될 것 같습니다.
만약 주어진 오브젝트가 null 일 경우, this는 global object를 지칭하게 됩니다.

>한편으로 다르게 생각해 보면, 오브젝트와 메서드가 분리되어 있는 형태라고도 생각해 볼 수 있을것 같습니다.

#### Synopsis

function.apply(thisobj, args)

#### Arguments
`thisobj`

The object to which function is to be applied. In the body of the function, thisobj becomes the value of the `this` keyword. If this argument is `null`, the global object is used.

The apply() method calls a function with a given this value, and arguments provided as an array (or an array-like object).

> Note: While the syntax of this function is almost identical to that of > call(), the fundamental difference is that call() accepts an argument list, while apply() accepts a single array of arguments.

> Note: When the first argument is undefined or null a similar outcome can > be achieved using the array spread syntax.

```js
var person = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}
var person1 = {
  firstName: "Mary",
  lastName: "Doe"
}
person.fullName.apply(person1);  // Will return "Mary Doe"
```


먼저 apply 함수를 살펴보면,
이 함수는 한 오브젝트에서 정의한 함수형 오브젝트를 다른 오브젝트에 대해서 사용할 수 있도록 한다.
첫번재 파라메터는 이 함수를 적용할 오브젝트이고, 다음 파라메터들은 이 함수를 호출할 때 사용되는 파라메터 어레이를 지정한다.

* Array(5) gives you an array with length 5 but no values, hence you can't iterate over it.
* Array.apply(null, Array(5)).map(function () {}) gives you an array with length 5 and undefined as values, now it can be iterated over.
* Array.apply(null, Array(5)).map(function (x, i) { return i; }) gives you an array with length 5 and values 0,1,2,3,4.
* Array(5).forEach(alert) does nothing, Array.apply(null, Array(5)).forEach(alert) gives you 5 alerts
* ES6 gives us Array.from so now you can also use Array.from(Array(5)).forEach(alert)
* If you want to initialize with a certain value, these are good to knows...
Array.from('abcde'), Array.from('x'.repeat(5))
or Array.from({length: 5}, (v, i) => i)   // gives [0, 1, 2, 3, 4]

[Difference between Array.apply(null, Array(x)) and Array(x)](https://stackoverflow.com/questions/28416547/difference-between-array-applynull-arrayx-and-arrayx)

There is a difference, a quite significant one.

The Array constructor either accepts one single number, giving the lenght of the array, and an array with "empty" indices is created, or more correctly the length is set but the array doesn't really contain anything

```js
Array(3); // creates [], with a length of 3
```

When calling the array constructor with a number as the only argument, you create an array that is empty, and that can't be iterated with the usual Array methods.

Or... the Array constructor accepts several arguments, whereas an array is created where each argument is a value in the array

```js
Array(1,2,3); // creates an array [1,2,3] etc.
```

When you call this

Array.apply(null, Array(3) )
It get's a little more interesting.

apply accepts the this value as the first argument, and as it's not useful here, it's set to null

The interesting part is the second argument, where an empty array is being passed in.
As apply accepts an array it would be like calling
```js
Array(undefined, undefined, undefined); // 여기서 캐스팅이 일어나버립니다!
```
and that creates an array with three indices that's not empty, but have the value actually set to undefined, which is why it can be iterated over.

TL;DR
The main difference is that Array(3) creates an array with three indices that are empty. In fact, they don't really exist, the array just have a length of 3.

Passing in such an array with empty indices to the Array constructor using apply is the same as doing Array(undefined, undefined, undefined);, which creates an array with three undefined indices, and undefined is in fact a value, so it's not empty like in the first example.

Array methods like map() can only iterate over actual values, not empty indices.

BTW, ECMAScript 2015 has Array.prototype.fill for this (also see MDN) so you can do:

```js
Array(9).fill(0);
```

즉, 위의 코드는 empty array를 지정된 값으로, 지정되어 있는 길이만큼 할당합니다.