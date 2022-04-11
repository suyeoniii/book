# 아이템 15: 동적 데이터에 인덱스 시그니처 사용하기

---

### 인덱스 시그니처

text

```ts
type Rocket = { [property: string]: string };
const rocket: Rocket = {
  name: "Falcon 9",
  variant: "v1.0",
  thrust: "4,94. kN",
};
```

> 💡 callout

### 요약

- text1
