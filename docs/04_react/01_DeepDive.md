---
sidebar_position: 1
---

# Deep Dive

> 2월 둘째주 글

### 리액트란

- UI를 만드는 JS 기반의 라이브러리
- 명령형보다는 선언형 기반. 데이터가 바뀜에 따라 렌더링. 코드 예측가능하고 디버그가 쉽다.
- 컴포넌트 기반. 캡슐형. html,css,js를 한꺼번에 작성
- jsx (js + xml)

### 기타

- 번들링은 rollup 같은거.

```ts
// entry.tsx
import React from "react";
import ReactDOM from "react-dom/client";
// import "core-js/stable";
// import "regenerator-runtime/runtime";

const $root = document.getElementById("root");
console.log(String.prototype.at);
console.log(Array.prototype.at);
console.log("hi2333");

if ($root) {
  const root = ReactDOM.createRoot($root);
  console.log(root);
  const a = ["123"].at(0);
  const b = Object.hasOwn({}, "3");
  console.log(a);
  console.log(b);

  const element = <h1>Hello, world</h1>;
  root.render(element);
}
```

```js
// rollup.config.js
import typescript from "rollup-plugin-typescript2";
import babel from "@rollup/plugin-babel";
import resolve from "@rollup/plugin-node-resolve";
import commonjs from "@rollup/plugin-commonjs";
import globals from "rollup-plugin-node-globals";

export default {
  input: "./hello.tsx",
  output: {
    file: "./hello.js",
    format: "es",
  },
  plugins: [
    resolve(),
    commonjs(),
    globals(),

    babel({
      babelHelpers: "bundled",
      presets: [
        [
          "@babel/preset-env",
          {
            targets: {
              ie: "10",
            },
            useBuiltIns: "usage",
            corejs: { version: "3.27", proposals: true },
          },
        ],
        "@babel/preset-typescript",
        "@babel/preset-react",
      ],
      extensions: [".js", ".jsx", ".ts", ".tsx"],
      include: ["**/*.tsx"],
      exclude: /node_modules/,
    }),

    // typescript({
    //   tsconfig: "tsconfig.json",
    // }),
  ],
};
```

```json
{
  "type": "module",
  "main": "hello.tsx",
  "dependencies": {
    "@babel/runtime-corejs3": "^7.20.13",
    "core-js": "^3.27.2",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@babel/core": "^7.20.12",
    "@babel/preset-env": "^7.20.2",
    "@babel/preset-react": "^7.18.6",
    "@babel/preset-typescript": "^7.18.6",
    "@rollup/plugin-babel": "^6.0.3",
    "@rollup/plugin-commonjs": "^24.0.1",
    "@rollup/plugin-node-resolve": "^15.0.1",
    "@types/glob": "^8.0.1",
    "@types/node": "^18.11.18",
    "@types/react-dom": "^18.0.10",
    "rollup": "^3.14.0",
    "rollup-plugin-node-globals": "^1.4.0",
    "rollup-plugin-typescript2": "^0.34.1",
    "typescript": "^4.9.5"
  }
}
```

- ⬆️ 코드 package.json 하나하나 컴파일방법까지 알아볼것
- 다시읽어볼링크 : https://chchoing88.github.io/ho_blog/hello-rollup.md/
- cs 409
- cs 1124
- https://tech.kakao.com/2020/12/01/frontend-growth-02/

### 추가로 공부하고 싶은 부분

- module system (cjs, esm...)
- rollup babel ...
