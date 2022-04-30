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

- 타입을 정의하는 방법에는 `type`과 `interface`를 이용하는 두가지 방법이 있음
- `class`로 정의할 수도 있지만, `class`는 자바스크립트 런타임의 개념이므로 조금 다름

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
- 클래스 구현(implements) 가능
  ```ts
  class StateT implements TState {
    name: string = "";
    capital: string = "";
  }
  class StateI implements IState {
    name: string = "";
    capital: string = "";
  }
  ```

### type과 interface의 차이점

- 유니온 타입은 type만 가능, 유니온 인터페이스는 없음!
  ```ts
  type AorB = "a" | "b";
  ```
- 인터페이스는 유니온 타입 같은 복잡한 타입을 확장하지는 못함
  -> 복잡한 타입을 확장하고 싶다면, 타입과 & 사용
  ```ts
  type NamedVariable = (Input | Output) & { name: string };
  ```

### type의 장점

- type을 이용한 튜플, 배열 타입
  ```ts
  type Pair = [number, number];
  type StringList = string[];
  type NamedNums = [string, ...number[]];
  ```
  -> interface도 비슷하게 구현은 가능하지만, 튜플의 함수는 사용하지 못함

### interface의 장점

- 보강(augment) 가능
  ```ts
  interface IState {
    name: string;
    capital: string;
  }
  interface IState {
    population: number;
  }
  const wyoming: IState = {
    name: "Wyoming",
    capital: "Cheyenne",
    population: 500_000,
  }; // 정상
  ```
  -> 같은 이름으로 interface를 정의하면, 속성이 합쳐져서 하나의 interface처럼 사용가능 `(=선언병합)`

### type과 interface 중 어떤 것을 사용해야할까?

- 복잡한 타입이라면 `타입 별칭` 사용
- 두가지 방법 모두 간단하게 표현 가능하다면 `일관성, 보강` 관점 고려
- API에 대한 타입 선언의 경우, 사용자가 새로운 필드를 병합할 수 있도록 `interface` 사용해야함
- 프로젝트 내부적으로 사용되는 타입의 경우, 선언 병합이 이루어지는게 바람직하지 않으므로 `type`을 사용해야함
