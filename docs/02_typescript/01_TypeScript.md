---
sidebar_position: 1
---

# TypeScript

> 1월 둘째주 글

## Typescript

- TypeScript는 JavaScript의 변형(variants). TypeScript는 JS의 구문이 허용되는, JavaScript의 상위 집합 언어
- JS는 웹 스크립트 언어. 간단히 사용하기 위함이 목적이었지만, 점점 사용성 증가하면서 JS의 이런 장점이 단점으로 작용하였다. 이런 점을 보완한 언어가 TS.
- 정적 타입 검사 : 프로그램을 실행시키지 않으면서 코드의 오류를 검출하는 것 / 어떤 것이 오류인지와 어떤 것이 연산 되는 값에 기인하지 않음을 정하는 것이 정적 타입 검사.

```js
// 어떤 것이 오류인지 정하는 것
"" == 0; // true
// 어떤 것이 연산되는 값에 기인하지 않음을 정하는 것
const obj = { width: 10 };
obj.height * obj.width; // NaN
```

- TypeScript는 JavaScript 코드의 런타임 특성을 절대 변화시키지 않습니다.
- 타입 시스템 자체는 프로그램이 실행될 때 작동하는 방식과 관련이 없습니다.
- TypeScript는 컴파일-타임 타입 검사자가 있는 JavaScript의 런타임입니다.
- TypeScript는 코드가 시간에 따라 변수가 변경되는 방식을 이해하며, 이러한 검사를 사용해 타입을 골라낼 수 있습니다.

```ts
typeof "string" | "boolean" | "number" | "undefined" | "function";
Array.isArray(type);
```

- 덕타이핑, 구조적타이핑 : 타입 확장에 대해 열려있다. 명확한 상속관계(A - B)를 지향하는 명목적 타이핑과 다르게 구조적 타이핑은 집합으로 포함한다는 개념을 지향.
