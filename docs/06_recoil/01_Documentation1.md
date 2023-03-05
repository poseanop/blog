---
sidebar_position: 1
---

# Documentaion1

> 주요개념 + 비동기

> 2월 4째주 글

## 리코일이란?

- 복잡한 앱을 위한 상태관리 라이브러리
  - 리액트만을 이용하면 공통 상위요소로 state를 끌어올려야하고, 그러면 불필요한 리렌더링 이슈 발생가능.
- `redux`, `zustand`가 중앙집중식 상태관리 (state와 action으로 구성되고 FLUX패턴을 이용함) 이라면
- `recoil`, `zotai` 는 atom으로 독립적이고 작은 상태관리 단위를 가짐.

## 철학

- 상태 변화의 흐름 : 뿌리(atoms) -> 순수함수(selectors) -> 컴포넌트
- **atoms** : 컴포넌트가 구독하는 최소단위
- **selectors** : atoms 상태값을 동기 또는 비동기 방식을 통해 변환. 파생된 상태(derived state)를 만들어낸다.
  - `파생된 상태` : 주어진 상태를 순수 함수에 전달해서 새로운 결과물을 만들어냄.
  - 다른 데이터에 의존하는 동적 상태데이터를 만들어내는 강력한 개념

## 비동기 데이터 쿼리

### 기본 사용법

- `selector`의 get 콜백을 `Promise` 를 리턴하게 하거나, `async/await` 키워드를 붙여주면 됨.

### fallback (Suspense)

- 비동기 데이터가 들어오기전에 fallback 처리는 `React.suspense`를 이용하면 된다.

```jsx
function MyApp() {
  return (
    <RecoilRoot>
      <React.Suspense fallback={<div>Loading...</div>}>
        <CurrentUserInfo />
      </React.Suspense>
    </RecoilRoot>
  );
}
```

### fallback (Suspense X)

- **useRecoilValueLoadable** : Suspense를 사용하지 않고 fallback 처리 가능하다.

### ErrorBoundary

- error 핸들링은 다음과같이 사용할 수 있다. (아직 함수형컴포는트는 지원하지 않는듯!)

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

### SelectorFamily (매개변수)

- 파생된 상태가 아닌 매개변수를 이용하고싶으면 `selectorFamily`를 사용할 수 있다.

```jsx
const userNameQuery = selectorFamily({
  key: "UserName",
  get: userID => async () => {
    const response = await myDBQuery({ userID });
    if (response.error) {
      throw response.error;
    }
    return response.name;
  },
});

function UserInfo({ userID }) {
  const userName = useRecoilValue(userNameQuery(userID));
  return <div>{userName}</div>;
}

function MyApp() {
  return (
    <RecoilRoot>
      <ErrorBoundary>
        <React.Suspense fallback={<div>Loading...</div>}>
          <UserInfo userID={1} />
          <UserInfo userID={2} />
          <UserInfo userID={3} />
        </React.Suspense>
      </ErrorBoundary>
    </RecoilRoot>
  );
}
```

### selector의 캐싱

- Data Flow Graph
- 이미 응답받은 atom, selector 값들은 캐싱되어있어서 재요청 보내지 않는다.

```jsx
const myDBQuery = ({ userID }) => {
  console.log("요청", userID);
  return new Promise((res, rej) => {
    const table = new Array(10).fill(0).map((item, i) => {
      return `${i}번째 유저`;
    });
    const index = (table.indexOf(userID) + 1) % 10;
    const nextIndex = (table.indexOf(userID) + 4) % 10;
    setTimeout(() => {
      res({
        name: userID,
        friendList: table.slice(index, nextIndex).map(item => {
          return item;
        }),
      });
    }, 1000);
  });
};

const currentUserIDState = atom({
  key: "CurrentUserID",
  default: "0번째 유저",
});

const userInfoQuery = selectorFamily({
  key: "UserInfoQuery",
  get: userID => async () => {
    const response = await myDBQuery({ userID });
    if (response.error) {
      throw response.error;
    }
    return response;
  },
});

const currentUserInfoQuery = selector({
  key: "CurrentUserInfoQuery",
  get: ({ get }) => get(userInfoQuery(get(currentUserIDState))),
});

const friendsInfoQuery = selector({
  key: "FriendsInfoQuery",
  get: ({ get }) => {
    const { friendList } = get(currentUserInfoQuery);
    return friendList.map(friendID => get(userInfoQuery(friendID)));
  },
});

function CurrentUserInfo() {
  const currentUser = useRecoilValue(currentUserInfoQuery);
  const friends = useRecoilValue(friendsInfoQuery);
  const setCurrentUserID = useSetRecoilState(currentUserIDState);
  return (
    <div>
      <h1>{currentUser.name}</h1>
      <ul>
        {friends.map(friend => (
          <li key={friend.id} onClick={() => setCurrentUserID(friend.name)}>
            {friend.name}
          </li>
        ))}
      </ul>
    </div>
  );
}

function MyApp() {
  return (
    <RecoilRoot>
      <React.Suspense fallback={<div>Loading...</div>}>
        <CurrentUserInfo />
      </React.Suspense>
    </RecoilRoot>
  );
}

export default MyApp;
```

### 병렬처리

- **waitForAll** : 비동기 병렬 요청 (Promise.all 같은거)
- **waitForNone** : 병렬처리는 동일한데, 결과값을 Loadable 객체로 받아 요소 하나하나 UI 핸들링이 가능하다

### atom의 default값을 비동기요청

- atom의 default 값을 selector로 참조하는 것도 일반적인 패턴임!!

### Refresh

- selector는 순수함수이므로, 일관된 값을 보장해야함. 그래서 요청했던 값은 캐싱처리하는데 필요에 따라 `refresh` 가 필요할 수도 있다.
  - `useRecoilRefresher` : selector의 모든 캐시를 제거하고 강제로 다시 selector를 재평가
  - `userInfoQueryRequestIDState` : selector를 업데이트하는 set Hooks에 ID를 부여하고 ID를 달리해서 요청하여 재평가 가능
  - selector 대신 atom 사용 ([setInterval](https://recoiljs.org/ko/docs/guides/asynchronous-data-queries#use-an-atom-atom-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)시 유용)

## 다시보기

- [pre-fetching](https://recoiljs.org/ko/docs/guides/asynchronous-data-queries#pre-fetching-%EB%AF%B8%EB%A6%AC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0)
- [흥미로운 예제](https://recoiljs.org/ko/docs/guides/asynchronous-data-queries#%EC%97%90%EB%9F%AC-%EB%A9%94%EC%8B%9C%EC%A7%80%EB%A5%BC-%ED%86%B5%ED%95%9C-%EC%BF%BC%EB%A6%AC-%EC%9E%AC%EC%8B%9C%EB%8F%84)
- 리코일 비동기와 useSWR react-query 비교
- [Document2](https://recoiljs.org/ko/docs/guides/atom-effects/)
