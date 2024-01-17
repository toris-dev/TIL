```javascript
let result = fetch(ServerURL)
result
  .then((response) => {
    if (response.ok) {
      // 요청 성공
    }
  })
  .catch((error) => console.error(error))

```
* 기존 `XMLHTTPRequest`를 대체하는 HTTP  요청 API
* ES6에 추가된 `Promise`를  리턴하도록 정의됨.
* 네트워크 요청 성공 시, `Promise`는 `Response` 객체를 resolve 한다.
* 네트워크 요청 실패 시, `Promise`는 에러를 `reject` 한다.

```javascript
let result = fetch(ServerURL)
	.then(response => {
		response.ok
		response.status
		response.statusText
		response.url
		response.bodyUsed
	})
```
* Response 객체는 결과에 대한 다양한 정보를 담는다.
* response.ok 는 HTTP Status Code가 200-299 사이면 true, 그 외 false 이다.
* response.status 는 HTTP Status Code를 담는다.
* repsonse.url은 요청한 URL 정보를 담는다.

### Body 메서드
```javascript
fetch(serverURL)
	.then(response => {
		return response.json()
	})
	.then(json => {
		console.log(`boddy : ${json}`)
	})
```
* response.json() 메서드는 얻어온 body 정보를 json으로 만드는 Promise를 반환한다.
* Promise가 resolve 되면 얻어온 body 정보를 읽는다.
* `response.text()`, `response.blob()`, `response.formData()` 등의 메서드로 다른 형태의 바디를 읽는다.

### POST 요청!!
```javascript
fetch(serverURL, {
	method: 'post',
	headers: {
		'Content-Type': 'application/json;charset=utf-8'
		Authentication: 'mysecret'	
	},
	body: JSON.stringify(formData)
})
	.then(response => {
		return response.json()
	})
	.then(json => {
		console.log(`POST 요청 결과: ${json}`)
	})
```
* fetch(url, options) 로, fetch 메서드 옵션을 넣는다.
* method 필드로 여러 요청 메서드를 활용한다.
* headers, body 필드를 활용해 서버에 추가 정보를 보낸다. 