### Server Component
함수 본문 상단에 "use server" 지시문을 사용하여 비동기 함수를 정의하여 서버 작업을 만든다.
"use server"은 이 기능이 서버에서만 실행된다.
```javascript
export default function ServerComponent() {
	async function myAction() {
		"use server"
		// ....
	}
}
```
위에 ServerComponent 는 전체가 서버 컴포넌트가 된다. 

그럼 클라이언트 컴포넌트에서 서버 액션을 사용하고 싶을 때는 어떻게 해야할까?  


### Client Component
파일 상단에 "use server" 지시문을 사용하여 별도의 파일에 서버 작업을 만든다.  
그런 다음 서버 작업을 클라이언트 구성 요소로 가져온다.

아래와 같이 lib/actions.ts 파일에 서버 액션을 모아두고 ClientComponent 에서 불러와서 사용하면 클라이언트 컴포넌트에서 서버 액션을 사용할 수 있다.  


![serveraction_클라이언트컴포넌트구조](/image/serveraction_클라이언트컴포넌트구조.png)

![](/image/serveraction_클라이언트컴포넌트%201.png)

![serveraction_serveraction_서버액션](/image/serveraction_서버액션%201.png)

아래와 같이 `form` 이라는 태그안에서 `action` 이라는 props를 줘서 **서버 액션**을 선언 할 수 있다.
또한 `form` 태그 안에 묶인 `input`, `button` 안에서도 `formAction` 이라는 props으로 줘서 **서버 액션**을 사용할 수 있다.

![serveraction사용법](/image/serveraction사용법.png)



### useOptimistic 사용
useOptimistic 은 Server Action이 호출되면 응답을 기다리지 않고 예상 결과를 반영하여 UI가 즉시 업데이트 된다.

하지만 이제 useOptimistic 으로 처리하라고 하는데 만약에 서버 액션이 처리가 잘못되었을 때 업데이트를 해버린다면 잘못되는게 아닌가 라는 생각이 든다...?  

response를 기다리기 보다 서버액션이 끝나기전에 UI를 업데이트해준다.  

원래는 베타버전으로 사용했었는데 14버전 부터 정식출시 하였다.
useTransition 훅은 상태 변화를 일으키는 함수의 우선순위를 낮추는 hook이다.  

아래 코드를 보면 2초 동안 sleep 되기 때문에 disabled={isPending}이 true 가 된다. 
이후에 백엔드에서 처리가 되고 나서 response 을 받고 revalidate가 되면 그 때 체크박스를 선택 할 수 있게 된다.



```typescript
"use client"

import React, { useOptimistic, useTransition } from "react"
import { Todo } from "./TodoList"
import { updateTodo } from "@/lib/actions"

const CheckBox = ({ todo }: { todo: Todo }) => {
  const [optimisticTodo, addOptimisticTodo] = useOptimistic(
    todo,

    (state: Todo, completed: boolean) => ({
      ...state,

      completed,
    })
  ) // const [isPending, startTransition] = useTransition();

  return (
    <input
      type="checkbox"
      checked={optimisticTodo.completed}
      id="completed"
      name="completed" 
      // disabled={isPending}
      onChange={async () => {
        addOptimisticTodo(!todo.completed)

        await updateTodo(todo)
      }}
      className="min-w-[2rem] min-h-[2rem]"
    />
  )
}
export default CheckBox
```

```typescript
// update
export async function updateTodo(todo: Todo) {
  const res = await fetch(`http://localhost:3001/todos/${todo.id}`, {
    method: "PUT",

    headers: {
      "Content-Type": "application/json",
    },

    body: JSON.stringify({
      ...todo,

      completed: !todo.completed,
    }),
  })

  await res.json() // 하는 이유는 CheckBox에서 disabled를 isPending값으로 설정을 하였기 때문에 트랜잭션 되지 않았을 때는 disabled 시킨다. 처리 된후 revalidate 시킨다.

  await sleep(2000)

  revalidatePath("/")
}

```
