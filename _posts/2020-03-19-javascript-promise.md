---
layout: default 
title: Javascript Promise
categories: [javascript]
tags: [javascript, promise]
---

# Promise

프라미스를 일반적으로 사용하는 방법은 잘 아실테니, 좀 더 진전된 토픽을 이야기해보도록 하겠습니다.

async function 하나를 생각해 봅시다. `createAudioFileAsync()` 라고 합시다. 

위 함수를 호출할 때, 성공했을 때는 successCallback, 실패했을 때는 falureCallback을 호출하도록 하기위해서 일반적이었던 방법(?)으로는 

```js
createAudioFileAsync(audioSetting, successCallback, failureCallback)
```
을 사용했더랬습니다.

요즘은 

```js
createAudioFileAsync(audioSetting).then(successCallback, failureCallback)
```

과 같이 호출합니다. 근데 ...

```js
createAudioFileAsync(audioSetting).then(res => {

})
```
를 살펴보면 `then`의 파라메터로는 successCallback 만 넘어가는 것이 아닌가요?

>`then()`은 콜백함수를 추가하는 함수입니다.  

then()은 여러번 호출될 수 있는데, 이렇게 되면 각 콜백이 순차적으로 호출되니, 아. 그렇다고 바로 위의 함수가 끝나고 호출되는 syncronous 한개 아니고, async 하게 호출됩니다. 

뭐, 이런거죠.

```js
doSomething()
.then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);
```
>위와 같이 then으로 추가되는 successfulCallback들은 doSomething이 success가 되었을때 모두 한번씩 순차적으로 호출됩니다. then이라고 해서 이전의 then callback 이 성공되어야 되는게 아니구요.


`then()`에 넘거가는 인자들은 옵셔널입니다. 그래서 `catch(failureCallback)`은 `then(null, failureCallback)`의 축약형입니다.


자. 그럼 Promise 를 한번 살펴보도록 하겠습니다.

Promise() 컨스트럭터는 promise로 감싸기 위한 즉, async화 하기 위한 함수를 파라메터로 넣어줍니다.

```js
var promiseObj = new Promise( tetherFunction );
```

여기에 파라메터로 넘어가는 ththerFunction 은 다음과 같은 시크너처를 가져야 합니다.

```js
tetherFunction(resolutionFunc, rejectionFunc)
```

tetherFunction이 async화 하기 위해서는 결과값을 리턴하는 대신, 성공적인 결과값일때는 resolutionFunc(value)와 같이, 에러일 경우에는 rejectionFunc(err)와 같이 처리해 줘야 합니다. 그렇게 하면 then과 catch함수로 등록된 callback함수들이 최종적으로 호출됩니다.


그러므로, Promise 함수는 어떻게 보면 선언형 프로그래밍과 일견 비슷해 보입니다. 

```js
new Promise((resolved, reject) => { ... }).then(res => {}).catch(err => {})
```

다 연결이 되어 있지 않으면 안되겠죠.


자 최종 정리를 해 봅시다.

Promise는 일반적인 syncronous한 함수를 async로 만들어 주기 위해 사용합니다.
sync한 함수를 async로 만들기 위해서는 

```js
tetherFunction(resolutionFunc, errorFunc) {
    ... 여기까지 기존 코드

    // 그리고, 마지막에 return 대신,
    if (success) 
        resolutionFunc(with result)
    else
        erroFunc(with error)
}
```

와 같이 만들어 줍니다. 그리고, then() catch()를 이용하여 async 한 tetherFunc의 결과를 처리해 줍니다.