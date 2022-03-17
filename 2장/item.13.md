# 아이템 13: 타입과 인터페이스의 차이점 알기

---

### 타입 정의

```ts
type Tstate = {
  name: string;
  capital: string;
};
```

```ts
interface Istate = {
  name: string;
  capital: string;
};
```

- 타입을 정의하는 방법에는 type과 interface를 이용하는 두가지 방법이 있음
- class로 정의할 수도 있지만, class는 자바스크립트 런타임의 개념이므로 조금 다름

### type과 interface의 유사점

- 명명된 타입은 둘다 차이없음
- 인덱스 시그니처 사용 가능
  ```ts
  type TDict = { [key: string]: string };
  interface IDict {
    [key: string]: string;
  }
  ```
- 함수 타입 정의 가능
  ```ts
  type TFn = (x: number) => string;
  interface IFn {
    (x: number): string;
  }
  const toStrT: TFn = (x) => "" + x;
  const toStrI: IFn = (x) => "" + x;
  ```
- 제너릭이 가능함
  ```ts
  type TPair<T> = {
    first: T;
    second: T;
  };
  interface IPair<T> {
    first: T;
    second: T;
  }
  ```
- 타입은 인터페이스를 확장할 수 있고, 인터페이스는 타입을 확장할 수 있음
  ```ts
  interface IStateWithPop extends Tstate {
    population: number;
  }
  type TStateWithPop = IState & { population: number };
  ```
