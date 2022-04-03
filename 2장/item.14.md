# 아이템 14: 타입 연산과 제너릭 사용으로 반복 줄이기

---

### 타입 반복 줄이기

**DRY(don't repeat yourself)원칙**을 타입에도 적용할 수 있음

1. 타입 중복이 많으므로, 타입에 이름을 붙여서 중복을 줄일 수 있음

```ts
function distance(a: { x: number; y: number }, b: { x: number; y: number }) {
  return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
}
```

```ts
interface Point2D {
  x: number;
  y: number;
}
function distance(a: Point2D, b: Point2D) {
  /* ... */
}
```

2. 한 인터페이스가 다른 인터페이스를 **확장**하게 해서 반복제거 가능

```ts
interface Person {
  firstName: string;
  lastName: string;
}

interface PersonWithBirthDate extends Person {
  birth: Date;
}
```

3. 타입 확장 - **인터섹션 연산자(&)** 사용

```ts
type PersonWithBirthDate = Person & { birth: Date };
```

```ts
interface State {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
  pageContents: string;
}
interface TopNavState {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
}
```

- TopNavState로 State를 확장하는 것보다 TopNavState를 State의 부분집합으로 만드는게 바람직함
  <br/>

```ts
type TopNavState = {
  userId: State["userId"];
  pageTitle: State["pageTitle"];
  recentFiles: State["recentFiles"];
};
```

- State를 인덱싱함

```ts
type TopNavState = {
  [k in "userId" | "pageTitle" | "recentFiles"]: State[k];
};
```

- State를 인덱싱한 코드에서 중복 제거

### 인덱싱 (Pair)

```ts
type Pick<T, K> = { [k in K]: T[k] };
type TopNavState = Pick<State, "userId" | "pageTitle" | "recentFiles">;
```

- Pick은 **제너릭 타입**이므로, 함수를 호출하듯이 사용가능

### 요약
