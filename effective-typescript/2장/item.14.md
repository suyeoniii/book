# 아이템 14: 타입 연산과 제너릭 사용으로 반복 줄이기

---

### 타입 반복 줄이기

1. **타입 중복이 많으므로, 타입에 이름을 붙여서 중복을 줄일 수 있음**
   **DRY(don't repeat yourself)원칙**을 타입에도 적용할 수 있음

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

2. **한 인터페이스가 다른 인터페이스를 **확장**하게 해서 반복제거 가능**

   ```ts
   interface Person {
     firstName: string;
     lastName: string;
   }

   interface PersonWithBirthDate extends Person {
     birth: Date;
   }
   ```

3. **타입 확장 - **인터섹션 연산자(&)** 사용**

   ```ts
   type PersonWithBirthDate = Person & { birth: Date };
   ```

4. **인덱싱**

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

5. **Pick**

   ```ts
   type Pick<T, K> = { [k in K]: T[k] };
   type TopNavState = Pick<State, "userId" | "pageTitle" | "recentFiles">;
   ```

   - Pick은 **제너릭 타입**이므로, 함수를 호출하듯이 사용가능

6. **keyof**

   ```ts
   interface Options {
     width: number;
     height: number;
   }
   interface OptionsUpdate {
     width?: number;
     height?: number;
   }
   ```

   - 두 인터페이스에는 공통으로 weight와 height가있지만, OptionsUpdate에서는 optional한 값임

   ```ts
   type OptionsUpdate = { [k in keyof Options]?: Options[k] };
   ```

   - keyof를 사용하여 key를 가져와서 optional을 붙여주면 중복이 해결됨

7. **typeof**

   ```ts
   const INIT_OPTIONS = {
     width: 640,
     height: 480,
     color: "#00FF00",
     label: "VGA",
   };
   interface Options {
     width: number;
     height: number;
     color: string;
     label: string;
   }
   ```

   이 경우, Options 인터페이스의 속성을 INIT_OPTIONS에서 가져오면 중복을 제거할 수 있음

   ```ts
   typeof Options = typeof INIT_OPTIONS;
   ```

8. **함수, 메서드 반환값 타입 명명**

   ```ts
   function getUserInfo(userId: string) {
     // ...
     return {
       userId,
       name,
       age,
       height,
       weight,
       favoriteColor,
     };
   }
   ```

   이런 경우 함수의 반환타입에 이름을 붙여주는 것이 좋은데, `ReturnType` 제너릭을 사용할 수 있음

   ```ts
   type UserInfo = ReturnType<typeof getUserInfo>;
   ```

   - getUserInfo함수의 반환타입을 UserInfo타입으로 명명

9. **제너릭 타입에서 매개변수 제한하기**
   모든 타입을 허용하지 않고, 특정 속성을 꼭 포함하도록 제한하고 싶은 경우 extends를 쓰면 됨

   ```ts
   interface Name {
     first: string;
     last: string;
   }
   type DancingDuo<T extends Name> = [T, T];

   const couple: DancingDuo<{first: string}> = [...] // error: Name에 필요한 last 속성이 없습니다.
   ]
   ```

   -> Name의 모든 속성을 포함하지 않고 있어서 에러발생

### 요약

- 타입에도 `DRY원칙` 적용하기
- 타입에 이름을 붙여서 반복피하기
- `keyof, typeof, 인덱싱, 매핑된 타입` 등 활용하기
- `제너릭타입`은 타입을 위한 함수와 같음
- `Pick, Partial, ReturnType`과 같은 제너릭 타입에 익숙해지기
