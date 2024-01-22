`memo`를 사용하면 컴포넌트의 props가 변경되지 않은 경우 리렌더링을 건너뛸 수 있습니다.

또한 React 에서는 useMemo() 라는 Hook 을 지원해줍니다.

---
### React.memo 란? 
동일한 `props`로 렌더링 한다면, `React.memo`를 사용하여 **성능 향상**을 누릴 수 있습니다.
**`memo`를 사용하면 `React`는 컴포넌트를 렌더링하지 않고 마지막으로 렌더링된 값과 재사용 됩니다.**

```js
const CommentItem = ({ title, content, likes, onClick }) => {
  function onRender(
    id, // 방금 커밋된 Profiler 트리의 "id"
    phase, // "mount" (트리가 방금 마운트가 된 경우) 혹은 "update"(트리가 리렌더링된 경우)
    actualDuration, // 커밋된 업데이트를 렌더링하는데 걸린 시간
    baseDuration, // 메모이제이션 없이 하위 트리 전체를 렌더링하는데 걸리는 예상시간
    startTime, // React가 언제 해당 업데이트를 렌더링하기 시작했는지
    commitTime, // React가 해당 업데이트를 언제 커밋했는지
    interactions // 이 업데이트에 해당하는 상호작용들의 집합
  ) {
    // 렌더링 타이밍을 집합하거나 로그...
    console.log(`actualDuration: ${title}: ${actualDuration}`);
  }

  const handleClick = () => {
    onClick();
    alert(`${title} 눌림`);
  };

  return (
    <Profiler id="CommentItem" onRender={onRender}>
      <div className="CommentItem" onClick={handleClick}>
        <span>{title}</span> <br />
        <span>{content}</span> <br />
        <span>{likes}</span>
      </div>
    </Profiler>
  );
};

export default memo(CommentItem);
```

전에 받았던 `props`를 새로 생성하지 않고 `memo()` 를 사용하여 전에 받았던 `props` 가 바뀌지 않았다면 기존의 memoization 해놓은 컴포넌트를 또 쓴다.

---
### useCallback

인라인 함수를 props로 넘겨주게 되면 새로 렌더링 될 때마다 새로운 함수를 만들어 주므로 모두 리렌더링 된다.

함수를 정의해서 넘겨줘도 상위에 컴포넌트가 리렌더링이 되서 결과는 전에 받았던 데이터도 리렌더 된다.

**핸들링 메서드**를 memoization 하여 넘겨주고 싶을 때는 useCallback 을 사용하면 된다.
```js
const Comments = ({ commentLists }) => {
  const handleChange = useCallback(() => {
    console.log("눌림");
  }, []);

  return (
    <div>
      {commentLists.map((comment) => (
        <CommentItem
          key={comment.title}
          title={comment.title}
          content={comment.content}
          likes={comment.likes}
          // onClick={() => console.log(`${comment.title} 눌림`)}
          onClick={handleChange}
        />
      ))}
    </div>
  );
};
```

---
### useMemo


## 정리
* 어떤 특정한 계산을 통해 만들어지는 특정 한 값을 memoization 은 `useMemo` 를 사용.
* 특정한 함수를 memoization 을 할 때는 `useCallback()`

* `React.memo` -> `HOC` / `Props` 비교하여 메모
* `Profiler` -> 리액트 성능 분석 도구
* callback -> `useCallback()`
* value -> `useMemo()`