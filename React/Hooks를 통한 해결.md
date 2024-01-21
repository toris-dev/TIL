### 개요

[함수형 컴포넌트의 한계](함수형%20컴포넌트의%20한계.md) 에서 봤다시피 어떻게 Hooks 으로 해결하였는지 알아보겠습니다.

---
### Too many hooks
- `useState<> - Component.state / setSta te`
	- 함수형 컴포넌트 내에서 상태 관리가 가능해짐
* useEffect<>ComponentcomponentDidMount / componentDidUpdate / componentWillUnmount Lifecycle Method 대체!!
* `useRef<> - React.createRef`
	* DOM Element 등, 렌더링 시점에 상관 없이 참조되는 유일한 값 저장하기
* `useContext<> - Context.Consumer / Component.contextType`
	* 여러 컴포넌트에서 공유할 수 있는 상태 만들기 (Scoped state?)
* `useCallback / useMemo` - 참조 유지
	* 자식 컴포넌트에 Props 전달할 때, 무조건적인 렌더링 발생 방지

