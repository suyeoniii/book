# 아이템 10: 객체 래퍼 타입 피하기

---

### string의 메서드

**기본형 타입**
`string, number, boolean, null, undefined, symbol, bigint`

- 기본형은 객체와 다르게 **메서드**를 가지지 않는다.
- 그런데 **string**의 경우 메서드를 가지고 있는것처럼 보임

```js
"hello".charAt(4); // o
```

string 타입에 메서드를 사용할 때 내부적으로는

1. `string`(기본형)을 `String`(객체)로 래핑
2. 메서드 호출
3. 래핑한 객체 버림

### 래핑한 객체를 버린다?

메서드를 사용하기 위해 기본형 타입(string)을 래퍼 타입(String)으로 래핑했다가, 메서드 사용 후 래핑된 래퍼 타입을 버리게 된다.

```js
x = "hello"; // x는 string 타입
x.language = "English";
console.log(x.language); // undefined
```

x는 string 타입이므로 language라는 속성을 줄 수 없지만, `속성을 줄 때 x는 String객체로 래핑`되었다가 다시 버려졌기에 `string타입인 x는 x.language를 가지고 있지 않게된다.`

### 래퍼 타입

`null, undefined`외에 모든 기본형타입은 래퍼타입을 가진다.
하지만 보통 래퍼 타입을 직접 생성할 필요는 없다.

> 💡 string은 String에 할당할 수 있지만, String은 string에 할당할 수 없다.
> 그래서 string타입을 매개변수로 받는 함수에 String객체를 전달하면 에러가 난다.

### 요약

- 기본형 값에 메서드를 제공하기 위해 객체 래퍼 타입이 어떻게 쓰이는지 이해하기
- 직접 사용하거나, 인스턴스를 생성하는 것은 피하기
- 객체 래퍼 타입은 지양하고, 대신 기본형 타입을 사용하기
