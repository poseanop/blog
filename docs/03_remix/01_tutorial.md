---
sidebar_position: 1
---

# Tutorial

> 1월 다섯째주

### 하이

- remix는 tailwind 를 사용한다.
- remix는 routes 디렉토리로 클라이언트사이드라우팅이 가능
- `useLoaderData` : router 함수 내에서 `loader` 함수를 export 해주어야함. `loader` 에서는 응답값을 `json()` 하고 return 해주어야함.
- remix는 prisma 를 사용한다.
  - prisma : db제어를 SQL말고 JS/TS사용할 수 있게 하는 패키지. 공부해야할듯 ^^;
- 동적경로는 $path.tsx로 설정가능
  - `loader` 파라미터로 slug 정보를 알 수 있음.
- `tiny-invariant` 를 이용한 TypeScript 처리 (`tiny-warning` 도 참고~~!)
- `marked` 패키지를 사용해서 마크다운 html 변환도 가능
  - [dangerouslySetInnerHTML](https://ko.reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) : XSS 위험을 상기시키기 위해 해당 속성으로 html을 전달!
- `Outlet` 를 사용하면 하위 routers directory를 해당 컴포넌트로 연다.
- router 함수 내에서 `action` 함수를 사용하면 formData 처리를 할 수 있다.
- 튜토리얼 여기까지 JS disable 해도 모든 동작이 작동함!! form데이터와 router 기반이어서 ㅎㅎ..;;
- https://github.com/poseanop/remix wip
