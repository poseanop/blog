---
sidebar_position: 2
---

# TypeScript Reference 정리

> 1월 셋째주 글

## Utility Type

### `Partial<Type>`

- 부분집합

```ts
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```

### `Required<Type>`

- 모든 프로퍼티가 필수

```ts
interface Props {
  a?: number;
  b?: string;
}

const obj: Props = { a: 5 };

const obj2: Required<Props> = { a: 5, b: 6 }; // 무조건만족해야함
```

### `Record<Keys,Type>`

- key, value 타입을 가진 객체

```ts
type Type = Record<"title" | "age", number>;
type Type2 = {
  title: number;
  age: number;
};
```

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
