### Client-Component vs Server-Component 구별
![](image/client%20component%20server%20component.png)
### generateStaticParams
generateStaticParams 함수는 해당 레이아웃 또는 페이지가 생성되기 전에 빌드 시간에 실행된다.
Revalidate(ISR) 중에는 다시 호출되지 않는다.
```javascript
export async function generateStaticParams() {
	const posts = await getPosts();
	return posts.map((post) => {
		slug: post.slug,	
	})
}
```

### Loading.tsx
Next.js 13에서는 React Suspense로 의미 있는 로딩 UI를 만드는 데 도움이 되는 새로운 파일 규칙 `loading.js` 를 도입했습니다. 
이 규칙을 사용하면 경로 세그먼트의 콘텐츠가 로드되는 동안 서버에서 즉시 로드 상태를  표시할 수 있으며 렌더링이 완료되면 새 콘텐츠가 자동으로 교체됩니다.
(스켈레톤 쉽게 구현)

### error.tsx
같은 폴더에서 error.js는 layout.js 및 template.js 안에 중첩됩니다.
page.js 파일과 그 아래의 모든 자식을 오류 경계로 래핑하지만 동일한 수준의 레이아웃이나 템플릿은 래핑하지 않습니다.
페이지 전체가 에러가 나지 않고 **해당 컴포넌트**만 에러가 나는것을 볼 수 있다.
 
![정상페이지|300](image/스크린샷%202024-01-15%20235406.png)
![에러페이지|300](image/스크린샷%202024-01-15%20235357.png)


### refresh()
```javascript
import { useRouter } from "next/navigation";
const router = useRouter()

router.refresh();
```
 위 와 같이 선언하여 refresh()를 호출하면 현재 경로가 서버에서 업데이트된 할일 목록을 새로고침하고 가져옵니다. 
 이는 브라우저 기록에 영향을 미치지 않지만 루트 레이아웃에서 아래로 데이터를 새로 고칩니다. 
 refresh()를 사용할 때 React 및 브라우저 상태를 모두 포함하여 클라이언트 측 상태가 손실되지 않습니다.
 -> full page refresh를 안해도 됩니다.
 
