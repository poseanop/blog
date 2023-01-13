---
sidebar_position: 1
---

# TypeScript 핸드북 정리

> 1월 둘째주 글

### Typescript란

- TypeScript는 JavaScript의 구문이 허용되는, JavaScript의 상위 집합 언어
- JS는 웹 스크립트 언어. 간단히 사용하기 위함이 목적이었지만, 점점 사용성 증가하면서 JS의 이런 장점이 단점으로 작용되었고, 이런 점을 보완하기 위해 TS가 탄생하였다.

### 정적 타입 검사

1.  프로그램을 실행시키지 않으면서 코드의 오류를 찾아준다
2.  해당 값이 연산 되는 값인지 체크해준다

```js
// 코드의 오류를 찾아준다. (JS에서는 참이지만 명백히 오류구문)
"" == 0; // true
// 연산되는 값인지 체크해준다. (undefined를 곱하는 것은 명백히 오류구문)
const obj = { width: 10 };
obj.height * obj.width; // NaN
```

### TypeScript 특징

- TypeScript는 JavaScript 코드의 런타임 특성을 절대 변화시키지 않습니다.
- 타입 시스템 자체는 프로그램이 실행될 때 작동하는 방식과 관련이 없습니다.
- JavaScript의 런타임동작을 모델링하는 타입시스템을 가진다. (컴파일타임에 체크)

### typeof (타입가드)

```ts
typeof "string" | "boolean" | "number" | "undefined" | "function";
Array.isArray(type);
```

### 덕타이핑, 구조적타이핑

- 명확한 상속관계(A - B)를 지향하는 **명목적 타이핑**과 다르게
- **구조적 타이핑**은 **집합으로 포함한다는 개념**을 지향.
- 덕분에 타입 확장에 대해 열려있다.

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

### TypeScript 주요옵션 (권장)

- `noImplicitAny` : any 를 사용하지 않도록 강제하는 옵션
- `strictNullChecks` : null과 undefined 를 처리를 강제하는 옵션

### as const

- 객체를 상수화 (리터럴타입으로)

```ts
const handleRequest = (url: string, method: "GET" | "POST") => {
  return;
};

const req = { url: "https://example.com", method: "GET" }; // as const;
handleRequest(req.url, req.method);
// req.method 는 string으로 추론되서 에러를 냄.
```

- req 객체 자체를 `as const` (리터럴 타입으로 변환) 해주어야함
- `method as "GET"` 타입단언 하는 방법도 있음

### false를 반환하는 타입

- 0
- NaN
- ""
- 0n
- null
- undefined

### never 타입

- `never` : 존재하지 않는 상태.

```ts
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}
// ---cut---
type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
  }
}
```

```ts
function fail(msg: string): never {
  throw new Error(msg);
}
```

### unknown 타입

- `unknown` 모든 값.
- any 타입과 유사하지만, unknown 타입에 어떤 것을 대입하는 것이 유효하지 않기 때문에 더 안전하다.

```ts
function f1(a: any) {
  a.b(); // OK
}
function f2(a: unknown) {
  a.b(); // ERROR
}
function f3(a: unknown) {
  return a; // OK. 대입시키지 않고 오류내지 않는 좋은 패턴
}
```

### 타입제한

- `extends` 로 타입제한

```ts
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}

// longerArray 의 타입은 'number[]' 입니다'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString 의 타입은 'alice' | 'bob' 입니다.
const longerString = longest("alice", "bob");
// 에러! Number에는 'length' 프로퍼티가 없습니다.
const notOK = longest(10, 100);
```

- **추론타입** 타입의 속성이 맞는지 체크

```ts
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a");
getProperty(x, "m"); // ERROR
```

### 타입 인수 명시

- TypeScript는 기본적으로 제네릭 호출에서 의도된 타입을 대체로 추론해 내지만, 항상 그렇지는 않습니다.

```ts
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
const arr = combine([1, 2, 3], ["hello"]); // error
const arr = combine<string | number>([1, 2, 3], ["hello"]); // 수동으로 타입 명시해야함
```

### Index signatures

- 유형 속성의 모든 이름을 미리 알지 못하지만 값의 형태는 알고 있는 경우

```ts
interface StringArray {
  [index: number]: string;
}

const getStringArray = () => {
  return ["3", "4"];
};

const myArray: StringArray = getStringArray();
const secondItem = myArray[1];
console.log(secondItem);
```

### keyof

- 객체 타입에서 객체의 키 값들을 숫자나 문자열 리터럴 유니언을 생성한다. 아래 타입 P는 “x” | “y”와 동일한 타입입니다.

```ts
type Point = { x: number; y: number };
type P = keyof Point; // "x" | "y"

type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish; // number
```

### ReturnType

- 함수 타입 을 받으면서 반환되는 타입을 제공

```ts
type Predicate = (x: unknown) => boolean;
type K = ReturnType<Predicate>; // boolean
```

- 타입이 아니라 실제 함수를 쓰고싶으면 `typeof` 키워드를 사용해야함.

```ts
function f() {
  return { x: 10, y: 3 };
}
type P1 = ReturnType<typeof f>; // typeof 로 함수의 타입을 넘겨주어야함.
type P2 = ReturnType<f>; // ERROR // 값을 넘기면 안됨!
```

### Indexed Access Types

- 타입의 특정 프로퍼티를 찾기 위해서 인덱싱된 접근 타입 을 사용할 수 있다.

```ts
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"];
```

- 리터럴 요소 타입 캡쳐링

```ts
const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
  { name: "Eve", age: 38 },
];

type Person = typeof MyArray[number];

type Age = typeof MyArray[number]["age"];
// Or
type Age2 = Person["age"];
```

### 조건부 타입

- 삼항연산자 사용

```ts
type MessageOf<T> = T extends { message: unknown } ? T["message"] : never;

interface Email {
  message: string;
}

interface Dog {
  bark(): void;
}

type EmailMessageContents = MessageOf<Email>;

type DogMessageContents = MessageOf<Dog>;
```

- 배열이면 요소값, 요소면 그대로리턴

```ts
type Flatten<T> = T extends any[] ? T[number] : T;

// Extracts out the element type.
type Str = Flatten<string[]>;

// Leaves the type alone.
type Num = Flatten<number>;
```

### 조건부 타입 내에서 추론

- infer : 직접 추출하지 않고 요소 타입을 추론

```ts
// qoduf
type Flatten<Type> = Type extends Array<infer Item> ? Item : Type;

// ReturnType<T> 에서 infer를 사용한다.
type GetReturnType<Type> = Type extends (...args: never[]) => infer Return ? Return : never;
```

### Mapped Types

- 중복을 피하기 위해 key 는 유지하되 value타입 값만 변경

```ts
type FeatureFlags = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
}; // 맵핑 타입

type FeatureOptions = OptionsFlags<FeatureFlags>;
/**
 * type FeatureOptions = {
    darkMode: boolean;
    newUserProfile: boolean;
   };
 * */
```

### Key Remapping via `as`

- 속성값 변경

```ts
type Getters<Type> = {
  [Property in keyof Type as `get${Capitalize<
    string & Property // string 머징이 필요함!!
  >}`]: () => Type[Property];
};

interface Person {
  name: string;
  age: number;
  location: string;
}

type LazyPerson = Getters<Person>;
/**
 * type LazyPerson = {
    getName: () => string;
    getAge: () => number;
    getLocation: () => string;
   }
 * /
```

- never 로 키 필터링

```ts
// 'kind' 프로퍼티를 제거합니다
type RemoveKindField<Type> = {
  [Property in keyof Type as Exclude<Property, "kind">]: Type[Property];
};

interface Circle {
  kind: "circle";
  radius: number;
}

type KindlessCircle = RemoveKindField<Circle>;
```
