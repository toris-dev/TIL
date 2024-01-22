### 개요
보통 전역 상태 관리는 라이브러리를 사용하여서 진행했는데 React 제공하는 Context 를 써본적이 없어 정리해보려고 합니다.

---
### 언제 써야 할까?
`context` 는 `react` 컴포넌트 안에서 전역적으로 데이터를 공유할 수 있도록 고안된 방법입니다.
웹 전체에서 사용되는 로그인한 유저, 테마, 선호하는 언어가 예시가 될 수 있습니다.

**하지만** 여러 레벨 걸쳐 props 넘기는 걸 대체하는 데에는 context 보다 Component [Composition](Composition.md) 이 더 간단한 해결책이 될 수 있습니다.


### 정리
* 컴포넌트 트리 제약 -> Props drilling의 한계 해소
* 재사용성 -> **Context를 사용하면 재사용하기 어려움**
* API -> createContext / Provider / Consumer
* useContext -> Consumer 대체

결론 -> 전역 상태 관리 Recoil, redux 를 사용하자!!