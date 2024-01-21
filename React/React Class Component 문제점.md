# 개요

리액트 에서는 클래스 컴포넌트의 문제점과 어떻게 극복을 하였는지 알아보겠습니다.
현재 리액트에서 클래스 컴포넌트를 쓰는 사람은 거의 없을겁니다.
함수형 컴포넌트를 사용하는데 그 이유를 알아봅시다!!

---

### 클래스 컴포넌트의 구조
react Component 인스턴스를 생성하는 클래스
![](../image/스크린샷%202024-01-21%20203349.png)

* `props`, `state` 재할당 시 값 비교 후 `render` 메서드 실행
* **비교 전략 제공** (shouldComponentRender, PureComponent)

---

### 한계
**인스턴스 재활용에 의한 부작용, 길어지는 코드**

**Class**
```js
class Gretting extends react.Component {
  showName = () => {
    alert(this.props.name) // 3초 후의 this.props.name은 클릭시점의 값과 다를 수 있다
  }
  handleShowName() {
    setTimeout(() => {
      this.showName()
    }, 3000)
  }
  render() {
    console.log(this)
    return (
      <>
        <h1>Hello, {this.props.name}</h1>
        <button onClick={this.handleShowName}>show name</button>
      </>
    )
  }
}

```

**Function**
실행 시점의 `props.name` 값을 바인딩한 채로 매 렌더링 마다 생성되는 `showName` 함수
```js
function SimplifiedGretting(props) {
  const showName = () => alert(props.name)
  const handleShowName = () => setTimeout(showName, 3000)

  return (
    <>
      <h1>Hello, {this.props.name}</h1>
      <button onClick={this.handleShowName}>show name</button>
    </>
  )
}

```

---
### What is this?
이벤트 핸들러에서 `React.Component` 기능을 사용할 때 생기는 문제
클래스 컴포넌트를 사용할 때는 this context를 생각하면서 작성해야 한다.

```js
class Gretting extends React.Component {
  showName() {
    alert(this.props.name)
  }
  
  handleShowName() {
    setTimeout(() => {
      this.showName() // 여기에서 this는 button 앨리먼트를 가리키기 때문에 error❗
    }, 3000)
  }

  render() {
    return (
      <>
        <h1>Hello, {this.props.name}</h1>
        <button onClick={this.handleShowName}>show name</button>
      </>
    )
  }
}

```

## 클래스 컴포넌트의 한계 요약
* 비동기 콜백 내 값 변조
	* 인스턴스 재활용으로 인한 비동기 함수내 `props`, `state` 변조 현상
* 많은 코드 작성량
	* 함수형 컴포넌트로 대체 가능한 불편하고 긴 문법
* `this` 바인딩
	* 브라우저 이벤트 핸들러 내 인스턴스 속성, 메서드 참조하면 `this` 값이 변경됨


---
