---
sidebar_position: 1
---

# TypeScript

> 1월 둘째주 글

### Typescript란

- TypeScript는 JavaScript의 구문이 허용되는, JavaScript의 상위 집합 언어
- JS는 웹 스크립트 언어. 간단히 사용하기 위함이 목적이었지만, 점점 사용성 증가하면서 JS의 이런 장점이 단점으로 작용되었고, 이런 점을 보완하기 위해 TS가 탄생하였다.

### 정적 타입 검사

- **정적 타입 검사**
  1.  프로그램을 실행시키지 않으면서 코드의 오류를 찾아준다
  2.  어떤 것이 오류인지와 어떤 것이 연산 되는 값인지 체크해준다

```js
// 코드의 오류를 찾아준다. (JS에서는 참이지만 명백히 오류구문)
"" == 0; // true
// 오류인지, 연산되는 값인지 체크해준다. (undefined를 곱하는 것은 명백히 오류구문)
const obj = { width: 10 };
obj.height * obj.width; // NaN
```

### TypeScript 특징

- TypeScript는 JavaScript 코드의 런타임 특성을 절대 변화시키지 않습니다.
- 타입 시스템 자체는 프로그램이 실행될 때 작동하는 방식과 관련이 없습니다.
- TypeScript는 컴파일타임 타입 검사자가 있는 JavaScript의 런타임입니다.
  - JavaScript의 런타임동작을 모델링하는 타입시스템을 가진다.
  - TypeScript는 코드가 시간에 따라 변수가 변경되는 방식을 이해하며, 이러한 검사를 사용해 타입을 골라낼 수 있습니다.

### typeof (타입가드)

```ts
typeof "string" | "boolean" | "number" | "undefined" | "function";
Array.isArray(type);
```

### 덕타이핑, 구조적타이핑

- 타입 확장에 대해 열려있다.
- 명확한 상속관계(A - B)를 지향하는 **명목적 타이핑**과 다르게
- **구조적 타이핑**은 **집합으로 포함한다는 개념**을 지향.

```ts
interface Point {
  x: number;
  y: number;
}

function returnPoint(p: Point) {
  return p;
}

const point = { x: 12, y: 26, z: 3 };
returnPoint(point);
```

### TypeScript 주요옵션

- `noImplicitAny` : any 를 사용하지 않도록 강제하는 옵션
- `strictNullChecks` : null과 undefined 를 처리를 강제하는 옵션

### as const

```ts
const handleRequest = (url: string, method: "GET" | "POST") => {
  return;
};

const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
// req.method 는 string으로 추론되서 에러를 냄.
```

1. `method as "GET"` 타입단언 해주거나
2. req 객체 자체를 `as const` (리터럴 타입으로 변환) 해주어야함

### false를 반환하는 타입

- 0
- NaN
- ""
- 0n
- null
- undefined
