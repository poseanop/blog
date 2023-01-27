---
sidebar_position: 1
---

# Tutorial

> 1월 다섯째주

### Short Tutorial

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

### Long Tutorial

- `<LiveReload />` 해줘야 live reloading 됨. 빠름
- app/root.tsx 에서 `links` 함수 + `<Link/>` 컴포넌트 추가 하면 CSS import 가능
- app 에 `.server` 파일이름이 들어가면 브라우저단 X
- [또보기](https://remix.run/docs/en/v1/tutorials/jokes#network-type-safety) : [assertion](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#assertion-functions), [zod](https://www.npmjs.com/package/zod)
- prisma 명령어
  - `npx prisma db push` : 데이터 유지하며 스키마 업데이트
  - `npx prisma db seed` : 시드 실행
  - `npx prisma studio` : 웹으로 DB 보기
- 더보기 CSRF 로그아웃 부분..!!
- `ErrorBoundary`
