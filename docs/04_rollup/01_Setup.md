---
sidebar_position: 1
---

# Setup

> 2월 둘째주 글

### 라이브러리

- 리액트
  - 데이터기반 렌더링, 컴포넌트기반, jsx형식의 선언형 UI라이브러리
- rollup
  - 번들링도구
  - /w babel, TS
- TypeScript
  - 정적타입 컴파일

### 세팅하기

```json
{
  "main": "index.js",
  "scripts": {
    "start": "npx http-server",
    "start:js": "rollup -cw"
  },
  "dependencies": {
    "core-js": "^3.27.2",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@babel/preset-env": "^7.20.2",
    "@babel/preset-react": "^7.18.6",
    "@babel/preset-typescript": "^7.18.6",
    "@babel/plugin-transform-runtime": "^7.19.6", // 트랜스파일링을 런타임에서 (트리쉐이킹, 전역오염 X)
    "@rollup/plugin-babel": "^6.0.3", // JS 트랜스파일링 도구 바벨 설정
    "@rollup/plugin-commonjs": "^24.0.1", // commonJS 모듈을 ES6 로 변환해줌
    "@rollup/plugin-node-resolve": "^15.0.1", // node_modules 경로에 있는 써드파티 모듈을 함께 번들링 해줌
    "@rollup/plugin-replace": "^5.0.2", // 번들되는 파일의 문자열 교체
    "@types/react": "^18.0.27",
    "@types/react-dom": "^18.0.10",
    "rollup": "^3.14.0"
  }
}
```

- [@rollup/plugin-node-resolve](https://github.com/rollup/plugins/tree/master/packages/node-resolve)
  - `preferBuiltins` : true 하면 `fs`, `path` 가져올시, 로컬로 가져옴. false와 `rollup-plugin-node-polyfills`를 같이 사용하면 얘를 가져옴
  - `resolveOnly` : resolve 할 모듈 선택
  - `moduleDirectories` : default는 ['node_modules'] . 추가가능
  - `browser` : true 하면 UMD 형식으로 빌드됨.
  - `mainFields` : `import * as D3 from 'd3'` 와 같은 npm 패키지를 가져올때 ['browser', 'module', 'main'] 어떤 순서대로 읽어올지 결정한다.
- [@rollup/plugin-commonjs](https://github.com/rollup/plugins/tree/master/packages/commonjs)
- [@rollup/plugin-replace](https://github.com/rollup/plugins/tree/master/packages/replace)
  - 보통 node기반의 process.env.NODE_ENV 를 "production", "development" 로 분기하여 사용
  - `preventAssignment` : true 하면 실제 변수에 할당하는 부분도 추가됨. (warning 때문에 함)
- [@rollup/plugin-babel](https://github.com/rollup/plugins/tree/master/packages/babel)
  - `exclude` : resolve에서 가져오기 때문에 babel에서는 exclude 설정.
  - `extensions` : 트랜스파일할 파일의 확장자.
  - `babelHelpers` : bundled | runtime. 런타임타임, 컴파일타임 언제 트랜스파일링할지 결정. [참고1](https://so-so.dev/tool/rollup/rollupjs-config/) [참고2](cs/409)
- [@babel/preset-env](https://babeljs.io/docs/en/babel-preset-env)
  - babel 은 그 자체로는 아무기능이 없고 preset과 plugin으로 작동한다.
  - `preset-env`는 브라우저폴리필, 최신문법 등을 사용하게 해주는 플러그인이다.
  - `useBuiltIns` : entry 로 corejs 를 import 해주거나, usage로 자동으로 감지하게 해주거나!
  - `corejs` : `useBuiltIns` 속성을 쓰면 외부에서 corejs를 임포트 한다는 말이다.
- [@babel/preset-typescript](https://babeljs.io/docs/en/babel-preset-typescript)
  - typescript를 쓰게 해줌.
  - `rollup-plugin-typescript2`(내부적으로 `tsc` 완벽지원) 과 비교하면 타입 export가 안됨. js->ts만 됨.
- [@babel/preset-react](https://babeljs.io/docs/en/babel-preset-react)
- [@babel/plugin-transform-runtime](https://babeljs.io/docs/en/babel-plugin-transform-runtime)
  - 이거를 사용하기 위해서는 `rollup-babel` 의 babelHelpers 옵션을 `runtime`으로 바꿔주어야함.
  - 라이브러리단 빌드면 `external: [/@babel\/runtime/]` 도 넣어주어야함.
  - `@babel/runtime-corejs3`, `@babel/runtime` 을 dependency로 설치
  ```json
  // 옵션추가
  {
    "corejs": {
      "version": 3,
      "proposals": true
    }
  }
  ```
  - 💡 또! plugin-transform-runtime usage-pure 올리고 untime-corejs3 디렉토리 안에 has-own, package.json도 추가해주면 됨.. 근데 런타임코어3에서 왜 디렉토리가 생성되는지는 파악안됨.
- `tsconfig.json` lib에 "ECMAScript 버전 추가", "dom" (document, console 사용)
- `tsconfig.json` jsx "react" 넣어야 리액트 컴파일타임에 인식
- polyfill 사용하려면 `core-js` 설치해야함.
- `package.json`
  - `main` : 노드와 함께 사용되는 패키지의 표준 필드. `cjs`
  - `module` : 번들링할 때 트리쉐이킹, 데드 코드 제거를 위해 esm을 import하도록 만든 옵션 `esm`
  - `browser` : 브라우저 친화 버전

### 엔트리

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

### 롤업 세팅 (번들)

```js
// rollup.config.js
import babel from "@rollup/plugin-babel";
import replace from "@rollup/plugin-replace";
import resolve from "@rollup/plugin-node-resolve";
import commonjs from "@rollup/plugin-commonjs";

export default {
  input: "./src/index.tsx",
  output: {
    file: "./index.js",
    format: "es",
  },
  plugins: [
    resolve(),
    commonjs(),
    replace({
      values: {
        "process.env.NODE_ENV": '"development"',
      },
      preventAssignment: true,
    }),
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
      exclude: /node_modules/,
    }),
  ],
};
```

### 롤업 세팅 (런타임)

```js
    babel({
      plugins: [
        [
          "@babel/plugin-transform-runtime",
          {
            corejs: {
              version: 3,
              proposals: true,
            },
          },
        ],
      ],
      babelHelpers: "runtime",
      presets: [
        [
          "@babel/preset-env",
          {
            targets: {
              ie: "10",
            },
          },
        ],
        "@babel/preset-typescript",
        "@babel/preset-react",
      ],
      extensions: [".js", ".jsx", ".ts", ".tsx"],
      include: ["**/*.tsx"],
      exclude: /node_modules/,
    }),
```

### 추가로 공부하고 싶은 부분

- module system (cjs, esm...), next generation (새포스팅)
- rollup, esbuild, webpack (새포스팅)
- 해결못한 `core-js-stable` 디렉토리 생성문제
