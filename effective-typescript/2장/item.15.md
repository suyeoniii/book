# 아이템 15: 동적 데이터에 인덱스 시그니처 사용하기

---

### 인덱스 시그니처

```ts
type Rocket = { [property: string]: string };
const rocket: Rocket = {
  name: "Falcon 9",
  variant: "v1.0",
  thrust: "4,94. kN",
};
```

- `[property: string]: string` 은 **인덱스 시그니처**

#### 단점

- 잘못된 key도 유효하다고 인식함
- 키가 꼭 필요하진 않음, {}도 유효함
- key마다 다른 타입을 가질 수 없음
- 타입스크립트의 자동완성 기능 동작 X

-> **interface**를 사용하면 해결됨

#### 활용

- **동적 데이터**를 표현할 때 사용함. e.g.행, 열 표현
- 객체의 속성을 할 수 없을 경우 유용. e.g. 열 이름을 미리 알지 못할 때

### 요약

- 런타임 때까지 객체의 속성을 알 수 없을 경우에 사용
- 안전한 접근을 위해 값 타입에 undefined를 추가할 수 있음
- 가능하면 interface, Record, 매핑된 타입과 같은 더욱 정확한 타입을 사용하는 것이 좋음
