## 개요

  

- 타입스크립트에서는 공통 타입을 변환을 용이하게 하기 위해서 유틸리티 타입을 제공합니다.
- 유틸리티 타입은 전역으로 사용 가능합니다.

- 종류
  - Partial<T> , Readonly<T>
  - Record<K,T> , Pick<T,K>
  - Omit<T,K>, Exclue<T,U>, Extract<T,U>
  - NonNullable<T>, Parameters<T>, ConstructorParameters<T>
  - ReturnType<T>, Required<T>

  

## Partial<T>, ReadOnly<T>

  

### Partial<T>

  

```ts

interface Todo {

  title: string;

  description: string;

}

  

// optional type을 쓰지 않고도 Partial<T> 로 가능

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {

  return { ...todo, ...fieldsToUpdate };

}

  

const todo1 = {

  title: "organize desk",

  description: "clear clutter",

};

  

const todo2 = updateTodo(todo1, {

  description: "throw out trash",

});

```

  

- 프로퍼티를 선택적으로 만드는 타입을 구성합니다.

- 주어진 타입의 모든 하위 타입 집합을 나타내는 타입을 반환합니다.

  

### Readonly

  

```ts

interface Todo {

  title: string;

}

  

const todo: Readonly<Todo> = {

  title: "Delete Inactive users",

};

  

todo.title = "Hello";

// error: Cannot assign to 'title' because it is a read-only property

```

  

- 프로퍼티를 읽기 전용(readonly)으로 설정한 타입을 구성합니다.

  

---

  

## Record<K,T>, Pick<T,K>, Omit<T,K>

  

### Record<K,T>

  

```ts

interface PageInfo {

  title: string;

}

  

type Page = "home" | "about" | "contact";

  

const x: Record<Page, PageInfo> = {

  about: { title: "about" },

  contact: { title: "contact" },

  

  // error: {subTitle: string} 은 할당하지 않았습니다.

  home: { subtitle: "home" },

  

  // error: Page 에 main 이라는 type은 할당하지 않았습니다.

  main: { title: "main" },

};

```

  

- 프로퍼티의 집합 K로 타입을 구성합니다.

- 타입의 프로퍼티들을 다른 타입에 매핑시키는데 사용합니다.

  

### Pick<T,K>

  

```ts

interface Todo {

  title: string;

  description: string;

  completed: boolean;

}

  

type TodoPreview = Pick<Todo, "title" | "completed">;

  

const todo: TodoPreview = {

  title: "Clean room",

  completed: false,

  

  // error: TodoPreview 에 description은 없습니다.

  description: "description",

};

```

  

- 프로퍼티 T에서 프로퍼티 K의 집합만을 선택해 타입을 구성합니다.

  

### Omit<T,K>

  

```ts

interface Todo {

  title: string;

  description: string;

  completed: boolean;

}

  

type TodoPreview = Omit<Todo, "description">;

  

const todo: TodoPreview = {

  title: "Clean room",

  completed: false,

  description: "description", // error : 'description'은 정의하지 않았습니다.

};

```

  

- T 의 모든 프로퍼티를 선택한 다음 K를 제거한 타입을 구성합니다.

  

---

  

## Exclude<T,U> , Extract<T,U>

  

```ts

type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c" 할당 가능

type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c" 할당 가능

  

// string | number 할당 가능

type T2 = Exclude<string | number | (() => void), Function>;

```

  

- T에서 U에 할당할 수 있는 모든 속성을 제외한 타입을 구성합니다.

  

### Extract<T,U>

  

```ts

type T0 = Extract<"a" | "b" | "c", "a" | "f">; // a

  

type T1 = Extract<string | number | (() => void), Function>; // () => void

```

  

- T에서 U에 할당 할 수 있는 모든 속성을 추출하여 타입을 구성한다.

  

---

  

## NonNullable<T>, Parameters<T>, ConstructorParameters<T>

  

### NonNullable<T>

  

```ts

type T0 = NonNullable<string | number | undefined>; // string | number

  

type T1 = NonNullable<string[] | null | undefined>; // string[]

```

  

- null과 undefined를 제외한 타입이다.

  

### Parameters<T>

  

```ts

declare function f1(arg: { a: number; b: string }): void;

type T0 = Parameters<() => string>; // []

type T1 = Parameters<(s: string) => void>; // [string]

type T2 = Parameters<<T>(arg: T) => T>; // [unknown]

type T3 = Parameters<typeof f1>; // [{a:number, b:string}]

type T5 = Parameters<any>; // unknown[]

type T6 = Parameters<never>; // never

type T7 = Parameters<string>; // 오류

type T8 = Parameters<Function>; // 오류

```

  

- 함수 타입 T의 매개변수 타입들의 튜플타입을 구성합니다.

  

### ConstructorParameters<T>

  

```ts

type T10 = ConstructorParameters<ErrorConstructor>; // [(string, | undefined)?]

type T1 = ConstructorParameters<FunctionConstructor>; // string[]

type T2 = ConstructorParameters<RegExpConstructor>; // [string, (string | undefined)?]

  

interface I1 {

  new (args: string): Function;

}

type T12 = ConstructorParameters<I1>; // [string]

  

function f1(a: T12) {

  a[0];

  a[1]; // error: [args:string] 의 인덱스 1이 없습니다.

}

```

  

- 생성자 함수 타입의 모든 매개변수 타입을 추론한다.

- 모든 매개변수 타입을 가지는 튜플 타입(T가 함수가 아닌 경우 `never`)을 생성한다.

  

---

  

## ReturnType<T>, Required<T>

  

### ReturnType<T>

  

```ts

declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>; // string

type T1 = ReturnType<(s: string) => void>; // void

type T2 = ReturnType<<T>() => T>; // {}

type T3 = ReturnType<<T extends U, U extends number[]>() => T>; // number[]

type T4 = ReturnType<typeof f1>; // {a:number, b: string}

type T5 = ReturnType<any>; // any;

type T6 = ReturnType<never>; // any

type T7 = ReturnType<string>; // 오류

type T8 = ReturnType<Function>; // 오류

```

  

- 함수 T의 반환 타입으로 구성된 타입을 생성한다.

  

### Required<T>

  

```ts

interface Props {

  a?: number;

  b?: number;

}

  
const obj: Props = { a: 5 };
const obj2: Required<Props> = { a: 5 };
// error: Property'b'is missing in type'{a:number;}

```

  

- T의 모든 프로퍼티가 필수적으로 설정된 타입을 구성한다.

- optional type 무시!!