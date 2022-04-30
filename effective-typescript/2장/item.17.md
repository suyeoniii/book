# 아이템 17: 변경 관련된 오류 방지를 위해 readonly 사용하기

---

### readonly는 언제 사용해야할까?

- 함수 내에서 매개변수 값을 변경하지 않아야할 때
- 호출하는 쪽에서 함수가 매개변수를 변경하지 않는다는 보장이 필요할 때

### readonly 배열

- 배열 요소를 읽을 수는 있지만 쓸 수는 없음
- 배열의 length를 읽을수는 있지만, 바꿀 수는 없음 <- 배열이 바뀌므로
- 배열을 변경하는 pop과 같은 메서드는 호출할 수 없음
- 변경 가능한 배열을 readonly 배열에 할당할 수 있음, 그 반대는 X

### readonly 사용하기

- 매개변수를 변경하지 않을거라면, readonly로 선언해주기
- 다른 라이브러리의 함수는 변경이 불가능하므로 타입 단언문 사용

### readonly 값 변경하기

```ts
let arr: readonly number[] = [1, 2, 3, 4, 5];
arr.push(6); // error
arr = arr.concat([1, 2, 3]); // ok
```

- 이 예제를 보면, `readonly는 변경 불가능한거 아닌가?` 싶을 수 있다
  여기서 concat은 원본을 수정하지 않고, 새 배열을 반환하므로 가능하다
- arr변수가 `가리키는 배열은 변경이 가능하지만, 배열 자체를 변경하지는 못하는 것`이다!

### Readonly제너릭

- readonly와 유사함, 객체에 사용됨
- readonly는 얕게 동작함
- readonly는 객체 내부의 속성에 대해서는 변경을 허용함
  ```ts
  interface Outer {
    inner: {
      x: number;
    };
  }
  const o: Readonly<Outer> = { inner: { x: 0 } };
  o.inner = { x: 1 }; // error: 읽기 전용 속성이기 때문에 inner에 할당할 수 없음
  o.inner.x = 1; // ok
  ```
  -> inner에만 readonly가 적용된 것

### 요약

- 함수가 매개변수를 수정하지 않는다면 readonly로 선언할 것
- readonly 사용시, 변경으로 인한 오류 및 변경이 발생하는 코드를 쉽게 찾을 수 있음
- const와 readonly의 차이 이해하기
  - const는 선언과 동시에 할당이 이루어짐
  - readonly는 원본을 변경하지 않아햐하지만, 새 값으로 대체하는 것은 가능함
- readonly는 얕게 동작함
  - 깊은 readonly를 사용하기 위해서는 ts-essentials라이브러리의 DeepReadonly제너릭 등을 이용할 수 있음
