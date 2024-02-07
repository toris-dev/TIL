### 개요
React 에서는 원래 React-Router 라는 외부라이브러리를 설치하여 라우팅을 진행했었습니다.

하지만 이제는 `Suspense` 컴포넌트를 이용하여 라우팅을 진행할 수 있게 되었습니다.

페이지 네비게이션을 트랜지션을 표시하는것이 좋습니다.

```js
import { Suspense, useState, useTransition } from "react"
import IndexPage from "./IndexPage.js"
import ArtistPage from "./ArtistPage.js"
import Layout from "./Layout.js"

export default function App() {
  return (
    <Suspense fallback={<BigSpinner />}>
            <Router />   {" "}
    </Suspense>
  )
}

function Router() {
  const [page, setPage] = useState("/") // page 경로
  const [isPending, startTransition] = useTransition()
  
  function navigate(url) {
    startTransition(() => {
      setPage(url)
    })
  }

  let content
  if (page === "/") {
    content = <IndexPage navigate={navigate} />
  } else if (page === "/the-beatles") {
    content = (
      <ArtistPage
        artist={{
          id: "the-beatles",
          name: "The Beatles",
        }}
      />
    )
  }
  return <Layout isPending={isPending}>      {content}    </Layout>
}

function BigSpinner() {
  return <h2>🌀 Loading...</h2>
}
```

* 트랜지션은 중단 가능하므로, 사용자는 다시 렌더링이 완료될때 까지 기다리지 않고 사용할 수 있습니다.
* 트랜지션은 원치 않는 로딩 표시를 방지하며, 사용자가 네비게이션 시 갑작스럽게 움직이는 것을 방지할 수 있습니다.