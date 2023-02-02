---
sidebar_position: 1
---

# Tutorial

> 2월 첫째주 글

### Short Tutorial [예제](https://github.com/poseanop/remix)

- remix는 tailwind 를 사용한다.
- remix는 routes 디렉토리로 클라이언트사이드라우팅이 가능
- `useLoaderData`
  - `loader` function에서 받은 json데이터를 가져온다.
  - `loader` : 라우팅 작업 이 호출된 후 데이터를 자동으로 재검증하고 최신 결과를 반환한다.
- remix는 prisma 를 사용한다.
  - prisma : db제어를 SQL말고 JS/TS사용할 수 있게 하는 패키지
- 동적경로는 $path.tsx로 설정가능
  - `loader` 파라미터로 slug 정보를 알 수 있음.
- `tiny-invariant` 를 이용한 TypeScript narrowing
- `marked` 패키지를 사용해서 마크다운 html 변환도 가능
  - [dangerouslySetInnerHTML](https://ko.reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) : XSS 위험을 상기시키기 위해 해당 속성으로 html을 전달!
- `Outlet` 를 사용하면 하위 routers directory를 해당 컴포넌트로 연다.
- router 함수 내에서 `action` 함수를 사용하면 formData 처리를 할 수 있다.
- 튜토리얼 여기까지 JS disable 해도 모든 동작이 작동함!! form데이터와 router 기반이어서.

### Long Tutorial [예제](https://github.com/poseanop/remix2)

- `<LiveReload />` 해줘야 live reloading 됨. 빠름 (개발모드만 작동)
- app/root.tsx 에서 `links` 함수 + `<Link/>` 컴포넌트 추가 하면 CSS import 가능
- app 에 `.server` 파일이름이 들어가면 브라우저단 X
- 컴파일단이 아니라 런타임단에서 api응답값 같은 경우의 validation type check 를 해주는 도구들도 많으니 알아볼 것!! 혹은 assertion 함수 사용
  - 내장서버에서는 prisma로 검증이 가능하지만, 그 외에 것들이 검증이 어려움!
  - [scheme-validation-layer](https://www.pumpkiinbell.com/blog/remote/scheme-validation-layer)
  - [TS - assertion](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#assertion-functions)
  - [zod](https://www.npmjs.com/package/zod)
- prisma 명령어
  - `npx prisma init --datasource-provider sqlite` : sqlite로 prisma를 초기화
  - `npx prisma db push` : 데이터 유지하며 스키마 업데이트
  - `npx prisma db seed` : 시드 실행
  - `npx prisma studio` : 웹으로 DB 보기
- [logout 기능](https://remix.run/docs/en/v1/tutorials/jokes#build-logout-action) : [CSRF](https://zzang9ha.tistory.com/341) 를 피하기 위해 `form: post` 방식으로 구현되었다.
  - 정확히 이해는 못했지만 `/logout` get으로는 로그아웃 안되고 항상 POST 요청으로만 로그아웃 가능하게끔 설정
- `ErrorBoundary` : 예상한 에러, 예상치 못한 에러 대응

### 추가로 공부하고싶은 부분

- prisma (새 포스팅)
- ts validation type check (TS 새포스팅)
