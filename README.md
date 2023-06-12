# React-LazyLoading-Suspense
리액트 로딩 지연과 서스펜스 연습

# 1. 지연 로딩 (Lazy Loading)
> 웹사이트 페이지의 리소스를 필요할 때 불러와주는 친구!

### 지연 로딩이란 ?
사용자가 페이지를 읽어들이는 시점에 필요한 리소스들이 우선 로드가 되고, 그 외 리소스들은 사용자가 필요로 할 때 추후에 로드하는 방법을 의미.

### 지연 로딩이 나오게 된 배경
일반적으로 웹 페이지가 열리면 페이지 전체가 렌더링 되어 모든 내용이 다운로드 됨. 브라우저가 웹 페이지를 캐싱한다는 점은 좋지만 다운로드 된 전체 내용을 사용자가 모두 확인할지는 미지수. 사용자가 확인하지 않는 리소스를 다운로드 한다는건 데이터 및 메모리 낭비. 하지만 사용자가 다운로드를 요청하는 일종의 행위를 통해 필요할 때 리소스를 다운로드한다면 이러한 낭비를 방지할 수 있음.

### 지연 로딩의 장점
1. 사용자에게 속도 측면에서 긍정적 인식 제공
: 사용자가 웹 사이트에 접속할 때 일부 리소스만 다운로드함으로써 로딩 속도가 빨라져 사용자에게 보다 빠르게 콘텐츠 제공 가능.최종적으로는 고객 이탈율을 줄일 수 있음.
2. 리소스 비용 절약
: 사용자가 필요로 하는 콘텐츠를 로딩하여 리소스(시간, 메모리) 비용을 아낄 수 있음.


### 지연 로딩의 역효과
1. 컨텐츠 버퍼링과 랜더링 속도에 영향을 줄 수 있음
: 지연 로딩의 일례인 무한 스크롤의 경우, 화면 맨 아래의 element가 감지되면 데이터를 불러오는데 리소스가 아직 다운로드 중일 때 사용자가 스크롤하게 되면 컨텐츠 로딩에 버퍼링이 발생. 이 경우 랜더링 속도에 안 좋게 작용할 수 있음.

2. 느린 이미지 로딩 속도
: 초기 웹 사이트를 로딩할 때 필요한 리소스만 다운로드 하기 때문에 추후 사용자가 필요로하는 리소스의 경우 페이지 레이이웃에 적합한 콘텐츠 크기를 가늠할 수 없음. 따라서 지연 로딩 이미지 랜더링 과정에서 로딩 속도 지연이 발생할 수 있음.

### 지연 로딩 최적화 사용법
1. 상대적으로 덜 중요한 콘텐츠에서 사용
: 모든 플랫폼에서 지연로딩을 제공하지 않는 점을 고려하여, 지연 로딩 사용시 에러 처리가 필수. 예상치 못한 역효과 발생시 사용자에게 좀더 나은 사용자 경험을 제공하도록 상대적으로 웹사이트에서 중요도가 낮은 콘텐츠에 지연 로딩 사용하기.

2. 검색엔진 최적화(SEO; Search Engine Optimization)에서는 지양
: 웹사이트 페이지의 데이터를 수집하고 분류(크롤링)하는 SEO에서는 일부 리소스만 로드하는 지연 로딩이 좋지 않은 영향을 줄 수 있음. 따라서 이 역시 중요하지 않은 컨텐츠에 지연 로딩 적용하기. 


# 2. 서스펜스 (Suspense)

### 서스펜스란 ?

비동기 작업 방식 중 하나로, 한 컴포넌트가 비동기적인 작업이 끝날 때까지 기다려야 할 때 해당 컴포넌트를 대신하여 다른 컴포넌트를 먼저 보여주고 비동기 작업이 끝나면 해당 컴포넌트를 보여주는 처리 방식.
# 2. 서스펜스 (Suspense)

### 서스펜스란 ?

비동기 작업 방식 중 하나로, 한 컴포넌트가 비동기적인 작업이 끝날 때까지 기다려야 할 때 해당 컴포넌트를 대신하여 다른 컴포넌트를 먼저 보여주고 비동기 작업이 끝나면 해당 컴포넌트를 보여주는 처리 방식.

```
const ProfilePage = React.lazy(() => import('./ProfilePage));

<Suspense fallback={<Spinner />}>
	<ProfilePage />
</Suspense>
```
[코드 출처](https://17.reactjs.org/docs/concurrent-mode-suspense.html)

ProfilePage 컴포넌트는 지연 로딩이 사용된 컴포넌트로, 비동기적인 작업이 완전히 끝날때까지 기다려야하는 상황.

ProfilePage이라는 자식 컴포넌트 비동기 작업이 처리되는 동안 서스펜서의 fallback이라는 값으로 주어진 Spinner 컴포넌트가 랜더링 됨.

리액트는 서스펜스의 자식 컴포넌트를 감지하고 있다가 해당 컴포넌트의 비동기 처리가 완료되면 리액트는 fallback 값의 Spinner가 아닌, 원래 노출하고 싶었던 컴포넌트로 리랜더링 수행.

즉, 서스펜스 컴포넌트는 비동기적인 컴포넌트를 자식으로 두며 그 자식 컴포넌트가 비동기적 작업을 진행하는 동안 fallback에 할당된 컴포넌트를 랜더링함. 그리고 리액트에게 비동기 작업하는 자식 컴포넌트를 알려줌. 비동기 작업이 끝나면 자식 컴포넌트를 랜더링하도록 알려줌.

### 서스펜스의 등장 배경

* 배경 1
스크립트가 위에서 아래로 읽히는 특성으로 인해 waterfall(계단식) 현상이 발생하여, 먼저 선언된 비동기 작업 코드 때문에 아래 코드들이 수행되지 못하고 사용자에게는 한정된 데이터만 보여지는 한계가 발생함. 이를 해결하고자 비동처리와 그 아리 코드가 동시에 진행될 수 있도록 하기 위해 서스펜스가 등장함.

```
function ProfilePage() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser().then(u => setUser(u));
  }, []);

  if (user === null) {
    return <p>Loading profile...</p>;
  }
  return (
    <>
      <h1>{user.name}</h1>
      <ProfileTimeline />
    </>
  );
}

function ProfileTimeline() {
  const [posts, setPosts] = useState(null);

  useEffect(() => {
    fetchPosts().then(p => setPosts(p));
  }, []);

  if (posts === null) {
    return <h2>Loading posts...</h2>;
  }
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.text}</li>
      ))}
    </ul>
  );
}

```
[코드 출처](https://17.reactjs.org/docs/concurrent-mode-suspense.html)
위와 같이, ProfilePage 함수의 fetchUser()라는 비동기적 작업으로 인해, ProfileTimeline 함수의 posts의 초기 리턴값인 'Loading posts...'가 보여지지 않아 사용자가 화면을 확인할 때 제한된 정보로 답답함을 느낄 수 있음. 이에 대한 해결책으로 아래와 같이 비동기적 작업들을 동시에 진행하는 방식 등장.

* 배경 2

```
function fetchProfileData() {
  return Promise.all([
    fetchUser(),
    fetchPosts()
  ]).then(([user, posts]) => {
    return {user, posts};
  })
}
```
하지만 이 방법 역시 fetchUser와 fetchPosts 둘중 한 작업이라도 시간이 걸리면 나머지 한 작업에 대한 데이터를 사용자가 기다려야 한다는 한계가 존재함. 이러한 한계에 대한 해결책으로 서스펜스 등장.

* 서스펜스 등장

```
const resource = fetchProfileData();

function ProfilePage() {
  return (
    <Suspense fallback={<h1>Loading profile...</h1>}>
      <ProfileDetails />
    </Suspense>
    <Suspense fallback={<h1>Loading posts...</h1>}>
        <ProfileTimeline />
      </Suspense>
  );
}

function ProfileDetails() {
  const user = resource.user.read();
  return <h1>{user.name}</h1>;
}

function ProfileTimeline() {
  const posts = resource.posts.read();
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.text}</li>
      ))}
    </ul>
  );
}
```
[코드 출처](https://17.reactjs.org/docs/concurrent-mode-suspense.html)
세스펜스는 비동기 작업들이 서로의 순서와 관계없이 랜더링 될 수 있도록 해줌. 먼저 실행되는 비동기 작업의 패칭을 시작하고, 그에 대한 대안의 fallback 값을 랜더링 한 후, 비동기 작업이 끝나면 비로소 해당 컴포넌트의 데이터를 랜더링 함.

서스펜스 역할 ? 기존의 waterfall 방식처럼 데이터 패칭 순서를 기재된 순서인 위에서부터 아래로 기다려야하는 방식이 아닌, 여러 비동기 작업들 중 먼저 끝나는 작업의 데이터를 먼저 보여줌!

### 서스펜스 사용시 주의할 점
서스펜스는 리액트에 자식 컴포넌트가 비동기적인 컴포넌트임을 알려줘야 함. 때문에 해당 컴포넌트는 비동기 데이터를 불러오는 함수를 써야하고, 해당 함수는 "특이한 형태"의 객체를 반환해야 함.

* 특이한 형태 ? "매서드 객체"
: 아래의 read()와 같이 비동기 처리의 status를 구분해야 함.
```
export const createResource = (promise) => {
	let status = "pending";
    let result;
    let suspender = promise.then(
    	(data) => {
          status = "success";
          result = data;
        },
        (err) => {
          status = "error";
          result = err;
        }
    );
    return {
    	read() {
          if (status === "pending") {
          	throw susender;
          } else if (status === "error") {
            throw result;
          }
          return result;
        },
     };
 };
```
