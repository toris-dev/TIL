* Promise 를 활용한 비동기 코드를 간결하게 작성하는 문
* async/await 문법으로 비동기 코드를 동기 코드처럼 간결하게 작성할 수 있다.
* async 함수와 await 키워드를 이용한다.
* await 키워드는 반드시 async 함수 안에서만 사용해야 한다.
* async로 선언된 함수는 반드시 Promise를 리턴한다.


### async 
```javascript
async function asyncFunc() {
  let data = await fetchData()
  let user = await fetchUser(data)
  return user
}

const asyncArrowFunc = async () => {
  let data = await fetchData()
  let user = await fetchUser(data)
  return user
}
```

async 함수는 function 키워드 앞에 async를 붙여 만든다.
async 함수 내부에서 await 키워드를 사용한다.
fetchData, fetchUser는 Promise를 리턴하는 함수이다.

### await 키워드 실행 순서
```javascript
async function asyncFunc() {
  let data1 = await fetchData1()
  let data2 = await fetchData2(data1)
  let data3 = await fetchData3(data2)
  return data3
}

function promiseFunc() {
  return fetchData1().then(fetchData2).then(fetchData3)
}

```
* await 키워드는, then 메서드 체인을 연결한것 처럼 순서대로 동작한다.
* 비동기 코드에 쉽게 순서를 부여한다.


### 에러처리

```javascript
function fetchData1() {
	return request()
		.then((response) => 
			response.requestsData))
		.catch(error => {
		// error 발생
	})
}
```
* Promise를 리턴하는 함수의 경우, 에러가 발생하면 catch 메서드를 이용하여 에러를 처리한다.
* catch 메서드를 사용하지 않는다면 async 함수에서 try-catch 구문을 이용하여 에러를 처리한다.

### 에러처리
```javascript
async function asyncFunc() {
  try {
    let data1 = await fetchData1()
    return fetchData2(data1)
  } catch (e) {
    console.error(`실패: ${e}`)
  }
}
```
* `try-catch` 구문으로 `async/await` 형태 비동기 코드 에러 처리가 가능하다.
* `catch` 절의 `e`는, `Promise`의 `catch` 메서드가 받는 반환 값과 동일하다.