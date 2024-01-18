Next.js 는 단순히 SSR 만 제공하는 프레임워크는 아닙니다.
그 외에도 훌륭한 컴포넌트와 훅을 지원하며 성능이 뛰어난 동적 웹 사이트를 만들 수 있으며 
최근에는 최적화에 대해 굉장히 신경쓰고 있습니다.

## 라우팅 시스템
원래 React 에서는 react-Router 라는 라우팅 라이브러리를 사용하여 외부 라이브러리를 설치를 하여 진행했었습니다. 
모든 페이지가 클라이언트에서만 만들어지고 렌더링 되는 것입니다.

Next.js는 다른 방법을 사용합니다. 바로 **파일시스템 기반 페이지**와 **라우팅**입니다.
pages/ 디렉터리 안의 모든 파일은 곧 애플리케이션의 페이지와 라우팅 규칙을 의미합니다.

pages/ 디렉터리 안의 .js, .jsx, .ts, .tsx 파일에서 `export` 한 리액트 컴포넌트라고 볼 수 있습니다.

**정적 라우팅과 동적 라우팅이 있습니다.**
Next.js 라우팅에는 보통 3가지 방법이 존재합니다.
1. **정적 라우팅은 pages/ 밑에 파일 이름으로 라우팅을 하게 되는 것입니다.**
2. **동적 라우팅은 파일 이름에 [slug].tsx 이런식으로 대괄호로 묶으시면 됩니다.**
   **그러면 [slug] slug안에 어떠한 값이 들어와도 경로 매개변수로 받습니다.**
3. **동적 라우팅인데 [...slug].tsx 이런식으로 spread operator 을 사용하면 뒤에 어떠한 경로 매개변수가 와도 인식을 하고 [...slug].tsx 페이지로 이동합니다.**

/posts/page/postId 이런식으로 되어있다면 디렉터리 구조를 만들기 힘들어지겠죠.
이럴 때는 posts/[...postId].tsx 이렇게 만드시면 됩니다.
![](../../image/스크린샷%202024-01-17%20232106.png)
위와 같은 구조로 되어있다면 localhost:3000/posts 와 
/posts/page/postId  가능
/posts/page  가능
/posts/hosts/32  가능
이렇게 뒤에 어떤 경로가 와도 [...postId].tsx 로 라우팅 됩니다. 

하지만 이렇게 막무가내로 들어가지게 되면 안되므로 직접 핸들링하여 리다이렉트 시킬수 있어야 합니다.

또한 spread operator, []대괄호 를 이용한 slug 파일은 한 디렉터리 당 하나씩만 만들 수 있으며 중복되면 개발 모드로 실행시켰을 경우 아래와 같은 에러가 떴습니다.
`Failed to reload dynamic routes: Error: You cannot use different slug names for the same dynamic path` 같은 에러를 보게 되며 slug 파일 둘 중 하나는 사용 못합니다.



또한 아래와 같은 디렉터리 구조에서는 **개발 모드**에서 실행하면
![](../../image/스크린샷%202024-01-17%20230959.png)
위에 폴더 구조는 localhost:3000/ 루트페이지 밖에 접근을 못하게 됩니다. 

루트페이지 말고 다른 페이지에 접근하게 된다면 404 error 페이지가 뜨게 됩니다.
![](../../image/스크린샷%202024-01-17%20231139.png)
이러한 에러 페이지도 직접 만들 수 있습니다.
`pages/404.tsx`  라는 파일을 만든 후 거기에 에러 컴포넌트를 만들면 
에러가 났을 경우 404.tsx 파일로 **리다이렉트** 됩니다.


---
## 페이지에서 경로 매개변수 사용하기
아까전에 봤던 [...postId].tsx 에 아무거나 입력을 하고 그 매개변수를 받아오겠습니다.
![](../../image/스크린샷%202024-01-17%20234212.png)

/posts/... 에서 입력을 아무렇게 해보니 이렇게 / 가 사라진 매개변수를 가져올 수 있었습니다.
![](../../image/스크린샷%202024-01-17%20234420.png)


또한 훅을 이용해서 가져올 수 도 있습니다.
![](../../image/스크린샷%202024-01-17%20234651.png)
훅을 이용해서 가져왔는데도 위와 같은 결과가 나오는 것을 확인했습니다.

즉, **클라이언트**, **서버**에서 경로 매개변수를 불어와서 사용할 수 있습니다.

---
## 클라이언트에서의 내비게이션
```javascript
import Link from "next/link"
import React from "react"

const Index = () => {
  return (
    <div>
      <Link href="/">Home</Link> <br />
      <Link
        href={{
          pathname: "/good/as/ds",
          query: {
            date: "2020-01-01",
            slug: "happy-new-year",
            foo: "bar",
          },
        }}
      >
        good/as/ds
      </Link>
    </div>
  )
}
export default Index
```
위와 같이 했을 때 Home Link 는 / 페이지로 이동시키고

good/as/ds 는 아래와 같은 쿼리 파라미터를 가지고 이동시킨다.
![](../../image/스크린샷%202024-01-18%20000823.png)

---

### router.push()
Next.js 에서는 `Link` 컴포넌트 대신 `useRouter` 라는 훅을 이용하여 다른 페이지로 이동 할 수 있습니다.
router.push('경로') 를 하게 되면 페이지로 이동이 된다.
아래와 객체로 **pathname** 과 **query**도 전달할 수 있다. 
```javascript
import Link from "next/link"
import { useRouter } from "next/router"
import React from "react"

const Index = () => {
  const handleRouter = () => {
   // router.push("/")
    router.push({{
	    pathname: "/good/as/ds",
          query: {
            date: "2020-01-01",
            slug: "happy-new-year",
            foo: "bar",
          },
    }})
  }
  const router = useRouter()
  return (
    <div>
      <Link href="/">Home</Link> <br />   
      <button onClick={handleRouter}>라우팅</button>  
    </div>
  )
}
export default Index

```

---

## 각종 컴포넌트

### Image
next.config.js 파일의 image 속성에 서비스 호스트명을 추가하면 이미지를 불러올 때 마다 Next.js 가 자동으로 최적화한다. 
```javascript
const nextConfig = {
  images: {
    domains: ["img1.daumcdn.net"],
  },
}
module.exports = nextConfig
```

Image 컴포넌트에는 4가지 필수 props이 있다.
* `src="url"`
* `width={200}`
* `height={300}`
* `alt="대체할 텍스트"`
```javascript
import Image from "next/image"
import React from "react"
const Index = () => {
  return (
    <div>
      <Image
        layout="responsive"
        width={500}
        height={500}
        src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbEjGAX%2FbtsDogEVjYz%2Fj6uDz8Y6DAigOSjhJnagv0%2Fimg.png"
        alt="대체할 이미지"
      />
    </div>
  )
}
export default Index
```
layout 속성에는 **fixed, responsive, intrinsic, fill** 이렇게 네 개의 값을 지정할 수 있다.

* `fixed`
	* html img 태그와 같습니다. 이미 크기를 지정하면 더 크거나 작은 화면에서도 이미지 크기를 조절하지 않고 지정한 대로 유지합니다.
*  `responsive`
	* fixed와 반대 방식으로 작동합니다. 
	* 화면 크기를 조절하면 그에 따라 이미지를 최적화 해서 제공합니다.
* `intrinsic`
	* fixed와 responsive를 절반씩 수용합니다. 
	* 크기가 작은 화면에서는 이미지 크기를 조절하지만 이미지보다 큰 화면에서는 이미지 크기를 조절하지 않습니다.
* `fill`
	* 부모 요소와 가로와 세로 크기에 따라 이미지를 늘립니다. layout에 fill을 지정한 경우 width와 height 속성값을 함께 지정할 수 없습니다. 
	* fill을 사용하는 것과 width/height 속성을 지정하는 것 중 하나만 가능합니다. 
	  둘다 사용하였을 경우 서버 Error 가 뜹니다.

---
### 메타데이터
**웹 애플리케이션에서는 메타데이터를 제대로 다루는 것이 정말 중요합니다.** 
페이스북에서는 오픈 그래프 라는 프로토콜을 사용해서 어떤 데이터를 카드에 표시할지 파악합니다.

소셜 네트워크나 웹 사이트에 필요한 정보를 주려면 웹 사이트에서 페이지에 몇 가지 메타데이터를 추가 해야 합니다.

**웹 사이트 자체는 이런 종류의 데이터가 없어도 잘 작동하지만 메타데이터를 제공하지 않으면 웹 사이트가 꼭 필요한 정보를 제공하지 않는다고 생각해서 검색엔진이 해당 페이지의 점수를 낮게 줄겁니다....**

어떠한 이벤트가 있음에 따라 메타데이터를 변경할 수 있는 것은 큰 메리트가 된다고 생각하며,
중요한 메타데이터들은 하나의 컴포넌트 안에 담아서 사용하는게 좋은 것 같습니다.


---
### _app.js와 _document.js 페이지 커스텀마이징

웹 어플리케이션에 따라 페이지 초기화 과정을 조절해야 하는 경우가 있습니다.
이 경우 페이지를 렌더링할 때마다 렌더링한 HTML을 클라이언트에 보내기 전에 특정 작업을 처리해야 합니다. 
Next.js 에서는 pages/ 안에 있는 _app.js 와  _document.js 로 이런 작업을 지정하고 처리합니다.

---


### _app.js

```javascript
import "@/styles/globals.css"
import type { AppProps } from "next/app"

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}

```
위와 같은 구조로 되어있으며 
MyApp 함수는 Component 라는 next.js 페이지 컴포넌트와 그 속성(pageProps)을 변환합니다.
`getServerSideProps()`, `getStaticProps()` 는 불러올 수 없습니다.

이 함수를 어디에서 쓰냐 예를 들면, 각 페이지마다 별도의 컴포넌트를 불러오지 않고 모든 페이지에서 챗봇을 사용할 수 있습니다.

또한 Provider를 사용하는 theme, react-query 를 사용할 때 여기에서 Provider로 감싸면 됩니다.

챗봇은 아래와 같이 컴포넌트를 정의하여 _app.js 에서 불러옵니다.

__참고__ `getInitialProps()` 는 레거시 API 이기 때문에 `getStaticProps()` 나 `getServerSideProps()`를 사용하는걸 권장합니다.
**`getInitialProps` 메서드를 사용할 수는 있지만 이 함수를 쓰면 사이트 최적화 기능을 사용할 수 없으며, 무조건 서버에서 모든 페이지를 렌더링하게 된다는 점을 명심해야 합니다.**

```javascript
import "@/styles/globals.css"
import type { AppProps } from "next/app"
import ChatBot from "@/components/ChatBot"
export default function App({ Component, pageProps }: AppProps) {
  return (
    <>
      <ChatBot />
      <Component {...pageProps} />
    </>
  )
}

```

---

### __document.js

Next.js 페이지 컴포넌트에서는 <head> <html> <body> 와 같은 기본적인 html 태그를 정의할 필요가 없습니다. 
앞에서 Head 컴포넌트를 사용하여 어떻게 <head> 태그를 수정하는지 알고 있습니다.
create-next-app 을 하면 _document.js 파일입니다.
```javascript
import { Html, Head, Main, NextScript } from "next/document"

export default function Document() {
  return (
    <Html lang="en">
            <Head />     
      <body>
                <Main />
                <NextScript />     
      </body>
 
    </Html>
   )
}
```


* Html 
	* Next.js 애플리에키션의 <html> 태그에 해당합니다. 여기에 lang과 같은 표준 html 속성들을 전달할 수 있습니다.
* Head
	* 애플리에키션의 모든 페이지에 대한 공통 태그를 정의할 때 이 컴포넌트를 사용할 수 있습니다. 이전에 살펴본 Head 컴포넌트와 다릅니다. 
	* 개념은 비슷하지만 Head는 반드시 웹 사이트의 모든 페이지에서 공통으로 사용되는 코드가 있을 때만 사용할 수 있습니다.
 * Main
	 * Next.js가 페이지 컴포넌트를 렌더링하는 곳입니다. 
	 * <Main> 외부의 컴포넌트는 브라우저에서 초기화되지 않기 때문에 페이지 간에 공통으로 사용되는 컴포넌트가 있다면 반드시 _app.js 파일에서 해당 컴포넌트를 사용해야 합니다.
  * NextScript
	  * Next.js는 클라이언트에 전송할 페이지를 렌더링 하고, 클라이언트에서 실행할 코드나 리액트 하이드레이션과 같은 작업을 처리할 수 있는 커스텀 스크립트를 끼워넣습니다. 
	  * NextScript는 이런 커스텀 자바스크립트가 위치하는 곳입니다.

_app.js 와 마찬가지로 _document 도  `getStaticProps()` 나 `getServerSideProps()`를 사용할 수 없습니다.



## 정리
**전체적인 PageRouter 가 어떤 구조로 동작하는지 알아봤으며** 
**이미지를 정확하게 제공하는 방법이나** 
**이동할 페이지에 필요한 데이터를 불러오는 방법과** 
**커스텀 메타데이터를 동적으로 생성하고 삭제하는 방법,**
**동적 라우트를 만들어서 사용자 경험을 향상 시키는 방법,**
**_app.js 파일과 _document.js 파일을 커스터마이징해서 별다른 노력 없이도 전체 애플리케이션에 사용자 인터페이스 등을 추가해서 상태를 유지하는 방법도 배웠습니다.**