# 아이템 18: 매핑된 타입을 사용하여 값을 동기화하기

---

### 예제

scatter plot을 그리기 위한 UI 컴포넌트를 작성한다고 했을 때 디스플레이를 제어할 속성이 필요함

```ts
interface ScatterProps {
  // data
  xs: numnber[];
  ys: number[];

  // display
  xRange: [number, number];
  yRange: [number, number];

  // events
  onClick: (x: number, y: number, index: number) => void;
}
```

- 데이터(xs, ys)나 디스플레이(xRange, yRange) 속성이 변경되면 다시 그려야함
- 이벤트 핸들러(onClick)가 변경되면 다시 그릴 필요 없음

## 최적화하기

---

### 1. 실패에 닫힌 접근법 (보수적 접근법)

```ts
function shouldUpdate(oldProps: scatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k]) {
      if (k !== "onClick") return true;
    }
  }
  return false;
}
```

- 새로운 속성이 추가되면 shouldUpdate함수가 차트를 다시 그림
- 차트가 정확하지만 너무 자주 그려질 가능성이 있음

### 2. 실패에 열린 접근법

```ts
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  return (oldProps.xs !== newProps.xs ||
  oldProps.ys !== newProps.ys ||
  oldProps.xRange !== newProps.xRange ||
  oldProps.yRange !== newProps.yRange ||
  oldProps.color !== newProps.color
  // (no check for onClick)
}
```

- 차트를 불필요하게 다시 그리는 단점 해결
- 실제로 차트를 다시 그려야할 경우 누락될 수 있음

### 3. 새로운 내용이 추가될 때 직접 추가하도록 하기

- 앞의 두가지 방법모두 이상적이지 않음

```ts
onClick: (x: number, y: number, index: number) => void

// 참고: 여기에 속성을 추가하려면,  shouldUpdate를 고치세요!
}
```

- 개발자가 스스로 체크하도록 하는 것보다는 타입 체커가 대신할 수 있게 하는 것이 좋음

### 4. 타입 체커가 동작하게 하기

```ts
const REQUIRES_UPDATE: { [k in keyof ScatterProps]: boolean } = {
  xs: true,
  ys: true,
  xRange: true,
  yRange: true,
  color: true,
  onClick: false,
};
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k] && REQUIRES_UPDDATE[k]) {
      return true;
    }
  }
  return false;
}
```

- ScatterProps와 REQUIRES_UPDATE가 동일한 속성을 가져야 한다는 것을 알려줌

> 💡 매핑된 타입은 두 객체가 같은 속성을 가지게할 때 유용함

### 요약

- 매핑된 타입을 사용해서 관련된 값과 타입을 동기화하도록 합니다
- 인터페이스에 새로운 속성을 추가할 때, 선택을 강제하도록 매칭된 타입을 고려해야 합니다.
