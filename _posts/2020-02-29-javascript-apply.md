---
layout: default
title: javascript apply syntax
categories: [programming, javascript]
tags: [javascript, syntax, apply]
---

# apply


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