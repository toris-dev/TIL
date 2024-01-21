### 개요
[Hooks를 통한 해결](Hooks를%20통한%20해결.md) 을 통해 함수형 컴포넌트에서 클래스 컴포넌트의 이점을 가져오면서 간결한 코드를 작성하는 방법을 알아봤는데요. 이번에는 Hooks 을 어떻게 사용하면 좋은지와 원리에 대해서 알아보겠습니다.

### Hooks, 어떻게 생겼을까
구현 방법 예상해보기!! ->

매번 함수형 컴포넌트가 실행될 때마다 이전 상태를 확인하고 기능을 수행한다.
실행된 Hooks 갯수가 달라져 실행 순서가 바뀔 경우 이전 상태를 비교하는 기준이 없어진다.
-> Hooks 생성 시점에 Unique key 를 제공했더라면?

`const [name, setName] = useState({key: 'NameForGreeting', state: 'students!'});`
-> Component 인스턴스를 내부에서 생성해 key를 컴포넌트 영역에 한정짓는다면?...

함수형 컴포넌트가 내부적으로 클래스 컴포넌트에 감싸져서 ->
인스턴스를 생성하고 .. ->
Hooks 는 사실 클래스 컴포넌트 API를 감싼 것처럼 변환될 뿐인.... ->

-> 매번 실행될 때마다 필요한 값(state,effets) 을 컴포넌트 외부에 순서대로 붙여자!! 
= Closure

```js
const SimpleReact = function () {
  // scope 1 (SimpleReact)
  // Closure value
  let _state // useState의 리턴에서는 _state의 "값"이 아닌 "참조"를 가진다.
  return {
    render(Component) {
      const component = Component()
      component.render()
      return component
    },
    useState(initialState) {
      // Scope 2 (useState)
      _state = _state || initialState // 초기값 없을 경우 할당
      function setState() {
        // Scope 3 (setState)
        _state = nextstate
      }
      return [_state, setState]
    },
  }
}

```

Closure는 함수안에서 함수가 실행된 시점에 필요한 값과 참조를 리턴되고 나서도 참조를 유지하는 것이 라고 불 수 있다. 