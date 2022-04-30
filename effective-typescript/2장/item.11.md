# 아이템 11: 잉여 속성 체크의 한계 인지하기

---

### 잉여 속성 체크

타입이 명시된 변수에 `객체 리터럴`을 할당할 때 해당 타입의 속성이 있는지, `그 외의 속성은 없는지` 확인하는 것

```ts
interface Room {
  numDoors: number;
  ceilingHeight: number;
}
const r: Room = {
  numDoors: 1,
  ceilingHeightFit: 10,
  elephant: "present", // error
};
```

`error`: 개체 리터럴은 알려진 속성만 지정할 수 있으며, 'Room'형식에 'elephant'이(가) 없습니다.

**구조적 타이핑 (아이템4)** 관점에선 변수r도 `Room`타입의 부분집합을 포함하므로 에러가 없어야함 (다음내용)

#### 임시 변수 사용

```ts
interface Room {
  numDoors: number;
  ceilingHeight: number;
}
const obj = {
  numDoors: 1,
  ceilingHeightFit: 10,
  elephant: "present",
};
const r: Room = obj; // error없음
```

마지막 줄의 obj는 객체리터럴이 아니므로 `잉여 속성 체크`는 하지 않고, `Room`타입의 `numDoors`, `ceilingHeight`속성만 체크합니다.

#### 약한 타입

```ts
interface LineChartOptions {
    logscale?: boolean;
    invertedAxis?: boolean;
    areaChart?: boolean;
}
const opts = { logScale: true };
const o: LineChartOptions: opts; // error
```

`error`: `{ logScale:boolean; }`유형에 `'LineChartOptions'`유형과 공통적인 속성이 없습니다.

- 약한 타입의 경우 값 타입과 선언 타입에 공통된 속성이 있는지 확인하는 별도의 체크를 수행함 (공통 속성 체크)
- 임시 변수를 제거해도 공통 속성 체크는 동작함

### 요약

- 잉여 속성 체크는 `임시 변수를 도입하면 잉여 속성 체크를 건너뛸 수 있다`는 한계가 있음
- 잉여 속성 체크는 오류를 찾는데엔 효과적이지만, `타입스크립트 타입 체커의 구조적 할당 가능성 체크와 역할이 다름.` 잉여 속성 체크와 일반적인 구조적 할당 가능성 체크를 구분할 수 있어야함
