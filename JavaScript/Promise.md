## Promise API

- Promise API는 비동기 API 중 하나이다.
- 태스크 큐가 아닌 잡 큐(Job queue, 혹은 microtask queue)를 사용한다.
- 잡 큐는 태스크 큐보다 우선순위가 높다.

```javascript
setTimeout(() => {
  console.log("setTimeout 타임아웃1");
}, 0);

Promise.resolve().then(() => console.log("Promise 1"));

setTimeout(() => {
  console.log("setTimeout 타임아웃2");
}, 0);

Promise.resolve().then(() => console.log("Promise 2"));

/* 
프로미스1 프로미스2
타임아웃1 타임아웃2
*/
```

### Promise

- 비동기 작업을 표현하는 자바스크립트 객체.
- 비동기 작업의 진행(pending), 성공(resolved), 실패(rejected) 상태를 표현한다.
- 비동기 처리의 순서를 표현할 수 있다.

### Promise 생성자

- `new Promise(callback)`
- `callback` 함수는 `(reolsve, reject)` 두 인자를 받는다.
- Promise가 성공했을 때 resolve를 호출한다.
- Promise가 실패 했을 때 reject를 호출한다.

```javascript
let promise = new Promise((resolve, reject) => {
  if (Math.random() < 0.5) {
    return reject("실패");
  }
  return resolve(10);
});
```

### Promise 메서드

- `then()` 메서드에 성공했을 때 실행할 콜백 함수를 인자로 넘긴다.
- `catch()` 메서드에 실패했을 때 실행할 콜백함수를 인자로 넘긴다.
- `finally()` 메서드는 성공/실패 여부와 상관없이 모두 실행할 콜백 함수를 인자로 넘긴다.
- `then(callback1, callback2)`로 callback1의 자리에 성공, callback2 의 자리에 실패 메서드를 인자로 넘길 수 있다.

```javascript
promise
  .then((data) => {
    console.log("성공 : ", data)
  })
  .catch((e) => {
    console.log("실패: ", e)
  })
  .finally(() => {
    console.log("promise 종료")
  })
```

### Promise 메서드 체인

- `then/catch` 메서드가 또 다른 `promise`를 리턴하여, 비동기 코드에 순서를 부여한다.
- 이렇게 동일한 객체에 메서드를 연결할 수 있는 것을 **체이닝** 이라 한다.
- 함수를 호출한 주체가 함수를 끝낸 뒤 자기 자신을 리턴하도록 하여 구현한다.
```javascript
promise
  .then((data) => {
    return fetchUser(data) // Promise를 리턴하는 함수
  })
  .then((user) => {
    console.log("User: ", user)
  })
  .catch((e) => {
    console.log("실패 : ", e)
  })
```

### Promise.all
* Promise.all 은 Promise의 배열을 받아 모두 성공 시 각 Promise의 각 resolved 값을 배열로 반환한다.
* 하나의 Promise라도 실패할 시, 가장 먼저 실패한 Promise의 실패 이유를 반환한다.
```javascript
Promise.all([promise1, promise2, promise3])
  .then((values) => {
    console.log("모두 성공:", values)
  })
  .catch((e) => {
    console.log("하나라도 실패: ", e)
  })
```
