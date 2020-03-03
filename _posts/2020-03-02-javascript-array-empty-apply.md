---
layout: default
title: javascript empty array and apply function
categories: [programming, javascript]
tags: [javascript, syntax, apply, array, empty-array, fill]
---
## 길이 n인 어레이의 생성 및 초기화

### when

when to generate `n-length` size array and `loop` with it.

### base

Array 클래스는 두개의 컨스트럭터를 가지고 있다.

```js
Array(n) // creates [], with a length of n.
// vs
Array(a, b, c) // creates [a, b, c], length is 3
```

위의 어레이중, empty array가 다른 함수의 파라메터로 넘어갈 때 캐스팅이 발생되는데, 
이 때 undefined 값으로 설정된 어레이로 캐스팅 된다.

```js
Array(n) => Array([undefined, undefined, ..., undefined(n th)])
```

이를 배경으로 깔고 아래의 글들을 보면 보다 이해하기 쉬울겁니다.  

>
>* Array(5) gives you an array with length 5 but no values, hence you can't iterate over it.
* Array.apply(null, Array(5)).map(function () {}) gives you an array with length 5 and undefined as values, now it can be iterated over.
* Array.apply(null, Array(5)).map(function (x, i) { return i; }) gives you an array with length 5 and values 0,1,2,3,4.
* Array(5).forEach(alert) does nothing, Array.apply(null, Array(5)).forEach(alert) gives you 5 alerts
* ES6 gives us Array.from so now you can also use Array.from(Array(5)).forEach(alert) => 강추!
* If you want to initialize with a certain value, these are good to knows...
Array.from('abcde'), Array.from('x'.repeat(5))
or Array.from({length: 5}, (v, i) => i)   // gives [0, 1, 2, 3, 4]

이와 관련한 글을 다음에 링크하였으니, 시간날 때 한번 더 보면 좋을것 같습니다.


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

console.log(...Array(3)); // => 이것과 같은 현상입니다.
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

