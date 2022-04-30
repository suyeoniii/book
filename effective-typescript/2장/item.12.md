# 아이템 12: 함수 표현식에 타입 적용하기

---

### 함수 Statement와 Expression

```ts
function rollDice1(sides: number): number {
  /* ... */
} // 문장 (statement)
const rollDice2 = function (sides: number): number {
  /* ... */
}; // 표현식 (expression)
```

- **함수 표현식**을 사용하는 것이 좋음
- 함수의 매개변수부터 반환값까지 전체를 **함수 타입**으로 선언할 수 있음

### 함수 타입

```ts
type DiceRollFn = (sides: number) => number;
const rollDice: DiceRollFn = (sides) => {
  /* ... */
};
```

- 매개변수, 반환값의 타입이 같은 함수를 같은 타입으로 통합할 수 있음

#### 공통 콜백 함수

- 라이브러리를 만든다면 **공통 콜백에 타입**을 제공해야함
- `e.g.` 함수의 매개변수에 명시하는 `MouseEvent` 타입 대신, 함수 전체에 적용할 수 있는 `MouseEventHandler` 타입을 제공함

#### 다른 함수의 타입 참조

```ts
const checkedFetch: typeof fetch = async (input, init) => {
  /* ... */
};
```

- `typeof fn`으로 다른 함수의 시그니처를 참조할 수 있음
