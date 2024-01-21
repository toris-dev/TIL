### this is hard
실행 환경에 따라 바뀌는 this

this = ...
* 전역 호출시 `window`
* 함수에서 호출 시 window, strict mode 일 경우 `undefined`
* 인스턴스 메서드로 호출 시 앞에 붙은 인스턴스가 `this`
	* -> `this.setState === [React.Component가 인스턴스].setState`
* 직접 바인딩 (`call`, `apply`, `bind`)
* 콜백의 경우 호출자가 결정하거나, this 를 인자로 직접 받음
	* => 브라우저 이벤트의 경우 엘리먼트 (`eventTarget`)
	* => `arr.forEach(callback(currentValue[,index[,arry]])[,thisArg])`

---

this의 값을 한 문맥에서 다른 문맥으 넘기려면 `call()`, `apply()` 을 사용해야 한다.

```js
// call 또는 apply의 첫 번째 인자로 객체가 전달될 수 있으며 this가 그 객체에 묶임
var obj = { a: "Custom" };

// 변수를 선언하고 변수에 프로퍼티로 전역 window를 할당
var a = "Global";

function whatsThis() {
  return this.a; // 함수 호출 방식에 따라 값이 달라짐
}

whatsThis(); // this는 'Global'. 함수 내에서 설정되지 않았으므로 global/window 객체로 초기값을 설정한다.
whatsThis.call(obj); // this는 'Custom'. 함수 내에서 obj로 설정한다.
whatsThis.apply(obj); // this는 'Custom'. 함수 내에서 obj로 설정한다.
```

```js
function add(c, d) {
  return this.a + this.b + c + d;
}

var o = { a: 1, b: 3 };

// 첫 번째 인자는 'this'로 사용할 객체이고,
// 이어지는 인자들은 함수 호출에서 인수로 전달된다.
add.call(o, 5, 7); // 16

// 첫 번째 인자는 'this'로 사용할 객체이고,
// 두 번째 인자는 함수 호출에서 인수로 사용될 멤버들이 위치한 배열이다.
add.apply(o, [10, 20]); // 34
```

---
### bind 메서드
ECMAScript5는 `Function.prototype.bind` 를 도입했습니다.
`f.bind(someObject)` 를 호출하여 f 와 같은 본문 코드와 범위를 가졌지만 this는 원본 함수를 가진 새로운 함수를 생성합니다.
새 함수의 `this`는 호출 방식과 상관없이 영구적으로 `bind()` 의 첫 번째 매개변수로 고정됩니다.

```js
function f() {
	return this.a;
}

var g = f.bind({a: "abcdef"})
console.log(g()); // abcdef

var h = g.bind({a: 'yoo'}); // bind는 한 번만 동작한다.
console.log(h()); // abcdef

var o = {a: 37, f: f, g: g, h: h};
console.log(o.a, o.f(), o.g(), o.h()); // 37 37 abcdef abcdef

/** 
결과값: abcdef abcdef 37 37 abcdef abcdef
*/
```


---
### this 바인딩 문제 회피하기

이벤트 핸들러의 this 바인딩 문제 회피하기

2번째 이미지 처럼 직접 this를 바인딩해서 사용하기도 하거나 arrow function 을 이용합니다.
![](../image/스크린샷%202024-01-21%20213317.png)

---
### 요약
this는 JS에서 함수형 프로그래밍에서는 필요가 없고 몰라도 됩니다. 
하지만 기초를 닦고 가는게 중요하므로 어떤 구조로 이루어져 있는지 복습도 할겸 정리해봤습니다.
