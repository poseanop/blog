---
sidebar_position: 2
---

# TypeScript Reference 정리

> 1월 셋째주 글

## Utility Type

### `Partial<Type>`

- 부분집합

### `Required<Type>`

- 모든 프로퍼티가 필수

### `Record<Keys,Type>`

- key, value 타입을 가진 객체

### `Pick<Type, Keys>`

- keys 에 대한 집합 생성

### `Omit<Type, Keys>`

- keys에 대해 제거한 타입 생성

### `Extract<Type, Union>`

- union 멤버 pick

### `Exclude<Type, ExcludedUnion>`

- union 멤버 omit

### `NonNullable<Type>`

- 타입에서 null, undefined 제거

## JSX

- `<>` 꺾쇠괄호 타입단언은 tsx에서 사용할 수 없다.
- `내장요소`(div..) 와 `컴포넌트`(Mycomponent...) 를 구분하기 위해서 컴포넌트는 항상 대문자를 써야함.

## Module

### 선택적 모듈 로딩

- `import id = require('')` 문으로 참조-제거 최적화 가능

```ts
declare function require(moduleName: string): any;
import { ZipCodeValidator as Zip } from "./ZipCodeValidator";
if (needZipValidation) {
  let ZipCodeValidator: typeof Zip = require("./ZipCodeValidator");
  let validator = new ZipCodeValidator();
  if (validator.isAcceptable("...")) {
    /* ... */
  }
}
```

### Ambient 모듈

- 구현을 정의하지 않는 선언

```ts
// node.d.ts
declare module "url" {
  export interface Url {
    protocol?: string;
    hostname?: string;
    pathname?: string;
  }
  export function parse(
    urlStr: string,
    parseQueryString?,
    slashesDenoteHost?
  ): Url;
}

// main.ts
/// <reference path="node.d.ts"/>
import * as URL from "url";
let myUrl = URL.parse("http://www.typescriptlang.org");
```

- Shorthand

```ts
declare module "hot-new-module"; // any로 결정됨
```

- Wildcard

```ts
// .text 일치하는 것들 cover
declare module "*.text" {
  const content: string;
  export default content;
}
```

### 추가로 공부하고싶은 부분

- Project Configuration (새로운 포스팅 예정)
