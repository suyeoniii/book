# 아이템 16: number 인덱스 시그니처보다는 Array, 튜플, ArrayLike를 사용하기

---

### 객체의 key는 문자열

- 복잡한 객체를 키로 사용하면, javascript내에서 객체가 문자열로 변환됨

  ```ts
  x[[1, 2, 3]] = 2;
  x; //{'1,2,3': 1}
  ```

- 숫자는 키로 사용할 수 없음, 숫자 사용시 문자열로 변환됨

  ```ts
  x = { 1: 2, 3: 4 };
  x; // {'1': 2, '3': 4}
  ```

#### 배열

배열도 객체이므로, **인덱스=key, key=문자열**이됨

```js
x = [1, 2, 3];
typeof x; // object
Object.keys(x); // ['0', '1', '2']
x[0]; // 1
x["0"]; // 1
```

- 타입스크립트에서는 **숫자 키**를 허용하고, 문자열 키와 다르게 인식함
- 타입은 런타임에 삭제되지만, 타입 체크 시점에서 오류를 잡을 수 있음
  ```ts
  x["1"]; // error
  x[1]; // ok
  ```

### for 문

##### for-in 문

- for-in 문에서의 key도 역시 string임
- for-in 문은 for-of문, for(;;)문보다 속도가 느리므로 지양할 것

##### for-of 문

- 인덱스가 필요하지 않을 때 사용할 것

##### forEach문

- 인덱스가 number타입으로 제공되기 때문에 인덱스의 타입이 중요할 때 사용

##### for(;;)문

- 반복문 중간에 멈춰야할 때 사용

### ArrayLike

어떤 길이를 가지는 배열과 비슷한 형태로 튜플을 사용하고 싶을 때 사용하는 타입

```ts
function checkedAccess<T>(xs: ArrayLike<T>, i: number): T {
  if (i < xs.length) {
    return xs[i];
  }
  throw new Error(`배열의 끝을 지나서 ${i}를 반환하려 했습니다`);
}
```

```ts
const tupleLike: ArrayLike<string> = {
  "0": "A",
  "1": "B",
  length: 2,
}; // 정상
```

- ArrayLike에서도 키는 여전히 문자열임

### 요약

- 배열은 객체이므로 **키는 숫자가 아니라 문자열**임
- 인덱스 시그니처로 사용된 number타입은 버그를 잡기 위한 타입스크립트 코드임
- 인덱스 시그니처에 number을 사용하기보다 **Array, 튜플, ArrayLike**타입을 사용하는 것이 더 좋음
