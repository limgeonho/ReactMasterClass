## Nomflix



- Route path는 항상 "/" 가 마지막에 와야함 안그러면 화면에 안보임... 항상 true이기 때문에

```typescript
// App.tsx

import { BrowserRouter as Router, Switch, Route } from "react-router-dom";
import Header from "./Components/Header";
import Home from "./Routes/Home";
import Search from "./Routes/Search";
import Tv from "./Routes/Tv";

function App() {
  return (
    <Router>
      <Header />
      <Switch>
        <Route path="/tv">
          <Tv />
        </Route>
        <Route path="/search">
          <Search />
        </Route>
        <Route path="/"> // "/"은 가장 마지막에 작성한다. => /로 시작하는 url은 전부 true로 해당되기 때문에 "/" 만 있는 url은 가장 마지막에 작성
          <Home />
        </Route>
      </Switch>
    </Router>
  );
}
export default App;
```



- useRouteMatch()

  => const homeMatch = useRouteMatch("/");

  => homeMatch 의 값은 해당 utl이 "/"와 일치하는 경우에 null이 아닌 object값으로 나타난다.

  => 하지만 "/" 같은 경우에는 "/" 로 시작하는 모든 url이 object가 있다고 나온다.

  => 따라서, "/" 는 isExact 값의 true / false에 따라 결정한다.

```typescript
// Header.tsx

const logoVariants = {
  normal: {
    fillOpacity: 1,
  },
  active: {
    fillOpacity: [0, 1, 0],
    transition: {
      repeat: Infinity,
    },
  },
};

function Header() {
  const homeMatch = useRouteMatch("/");
  const tvMatch = useRouteMatch("/tv");
  return (
    <Nav>
      <Col>
        <Logo
          variants={logoVariants} // logoVariants
          whileHover="active" // hover시 값
          initial="normal" // 시작 initial 값
          xmlns="http://www.w3.org/2000/svg"
          width="1024"
          height="276.742"
          viewBox="0 0 1024 276.742"
        >
          <motion.path d="..." />
        </Logo>
        <Items>
          <Item>
            <Link to="/">Home {homeMatch?.isExact && <Circle />}</Link>
          </Item>
          <Item>
            <Link to="/tv">Tv Shows {tvMatch && <Circle />}</Link>
          </Item>
        </Items>
      </Col>
```



- `<Link>`는 `<a>`와 다르게 페이지가 새로고침되지 않는다.

```typescript
<Item>
  <Link to="/">Home {homeMatch?.isExact && <Circle />}</Link>
</Item>
<Item>
  <Link to="/tv">Tv Shows {tvMatch && <Circle />}</Link>
</Item>
```



- `{tvMatch && <Circle />}` 

  => tvMatch의 값이 있을경우에 `<Circle />`을 표시하겠다.



- framer motion의 layoutId 옵션

  => react가 서로 다른 component로 인식하고 있는 component를 하나의 layoutId로 묶어 주면 알아서 자연스러운 animation을 적용해 준다.

```typescript
// Header.tsx

<Item>
	<Link to="/">
		Home {homeMatch?.isExact && <Circle layoutId="circle" />}
	</Link>
</Item>
<Item>
	<Link to="/tv">
		Tv Shows {tvMatch && <Circle layoutId="circle" />}
	</Link>
</Item>
```



- toggle 스위치를 누를때 마다 돋보기 아이콘 왼쪽으로 input창이 나타났다가 사라지게 만들기

```typescript
// Header.tsx

const Input = styled(motion.input)`
  transform-origin: right center; // transform-origin => 시작되는 점 : right -> left
  position: absolute;
  left: -150px;
`;

const logoVariants = {
  normal: {
    fillOpacity: 1,
  },
  active: {
    fillOpacity: [0, 1, 0],
    transition: {
      repeat: Infinity,
    },
  },
};

function Header() {
  const [searchOpen, setSearchOpen] = useState(false);
  const homeMatch = useRouteMatch("/");
  const tvMatch = useRouteMatch("/tv");
  const toggleSearch = () => setSearchOpen((prev) => !prev); // useState를 이용한 toggle

  return (
    <Nav>
      <Col>
        <Logo
          variants={logoVariants}
          whileHover="active"
          initial="normal"
          xmlns="http://www.w3.org/2000/svg"
          width="1024"
          height="276.742"
          viewBox="0 0 1024 276.742"
        >
          <motion.path d="..." />
        </Logo>
        <Items>
          <Item>
            <Link to="/">
              Home {homeMatch?.isExact && <Circle layoutId="circle" />}
            </Link>
          </Item>
          <Item>
            <Link to="/tv">
              Tv Shows {tvMatch && <Circle layoutId="circle" />}
            </Link>
          </Item>
        </Items>
      </Col>
      <Col>
        <Search>
          <motion.svg
            onClick={toggleSearch}
            animate={{ x: searchOpen ? -180 : 0 }}
            transition={{ type: "linear" }}	// linear 하게 변화함
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fillRule="evenodd"
              d="..."
              clipRule="evenodd"
            ></path>
          </motion.svg>
          <Input
            transition={{ type: "linear" }} // linear 하게 변화함
            animate={{ scaleX: searchOpen ? 1 : 0 }} // 변화 후 상태 animate ~
            placeholder="Search for movie or tv show..."
          />
        </Search>
      </Col>
    </Nav>
  );
}

export default Header;
```



- 돋보기를 누르면 toggle의 상태가 변하면서 input창이 나온다

```typescript
function Header() {
  const [searchOpen, setSearchOpen] = useState(false);
  const homeMatch = useRouteMatch("/");
  const tvMatch = useRouteMatch("/tv");
  const toggleSearch = () => setSearchOpen((prev) => !prev); // useState를 이용한 toggle
	...
	
    <Input
     transition={{ type: "linear" }} // linear 하게 변화함
     animate={{ scaleX: searchOpen ? 1 : 0 }} // 변화 후 상태 animate ~
     placeholder="Search for movie or tv show..."
    />
     ...
```



- useAnimation()

  => animate를 직접 component의 props로 부터 실행하는 것이 아니라 함수의 코드로 실행하고 싶을때 사용하는 react hook

```typescript
// Header.tsx

function Header() {
  const [searchOpen, setSearchOpen] = useState(false);
  const homeMatch = useRouteMatch("/");
  const tvMatch = useRouteMatch("/tv");
  // const toggleSearch = () => setSearchOpen((prev) => !prev);
  const inputAnimation = useAnimation();
  const toggleSearch = () => {	// animate를 직접 component의 props에 적용하던 것을 함수의 코드로 적용
    if (searchOpen) {
      inputAnimation.start({
        scaleX: 0,
      });
    } else {
      inputAnimation.start({
        scaleX: 1,
      });
    }
    setSearchOpen((prev) => !prev);
  };
	...
	
<Input
 animate={inputAnimation} // useAnimation()을 적용
 transition={{ type: "linear" }}
 placeholder="Search for movie or tv show..."
/>
	...
```



- useViewportScroll()

  => 페이지로부터 scroll이 얼마나 되었는지에 대해 return 해줌

  => scroll 했을 때 특정 높이부터 다른 효과를 주고 싶을 때 사용

  => useEffect(() => {}, [])를 통해 useViewportScroll()값을 감시하고 특정 조건에 맞을 때 

  => useAnimation() 적용

```typescript
// Header.tsx

const navVariants = { // variants 설정
  top: {
    backgroundColor: "rgba(0, 0, 0, 0)",
  },
  scroll: {
    backgroundColor: "rgba(0, 0, 0, 1)",
  },
};

function Header() {
  const [searchOpen, setSearchOpen] = useState(false);
  const homeMatch = useRouteMatch("/");
  const tvMatch = useRouteMatch("/tv");
  const inputAnimation = useAnimation();
  const navAnimation = useAnimation(); // animation 함수화
  const { scrollY } = useViewportScroll(); // animation 함수화
  const toggleSearch = () => {
    if (searchOpen) {
      inputAnimation.start({
        scaleX: 0,
      });
    } else {
      inputAnimation.start({ scaleX: 1 });
    }
    setSearchOpen((prev) => !prev);
  };
  useEffect(() => { // useEffect를 통해 감시 대상을 설정하고 tracking 
    scrollY.onChange(() => {
      if (scrollY.get() > 80) { // 특정 조건에 맞을 때
        navAnimation.start("scroll"); // 해당 값 실행
      } else {
        navAnimation.start("top");
      }
    });
  }, [scrollY, navAnimation]); // 감시 대상

  return (
    <Nav variants={navVariants} animate={navAnimation} initial={"top"}> // animation 설정
      <Col>
        <Logo
          variants={logoVariants}
          whileHover="active"
          animate="normal"
          xmlns="http://www.w3.org/2000/svg"
          width="1024"
          height="276.742"
          viewBox="0 0 1024 276.742"
        >
          <motion.path d="..." />
        </Logo>
        <Items>
          <Item>
            <Link to="/">
              Home {homeMatch?.isExact && <Circle layoutId="circle" />}
            </Link>
          </Item>
          <Item>
            <Link to="/tv">
              Tv Shows {tvMatch && <Circle layoutId="circle" />}
            </Link>
          </Item>
        </Items>
      </Col>
```



- 원하는 그림을 background-image로 넣을 때 + 음영

```typescript
// Home.tsx

const Banner = styled.div<{ bgPhoto: string }>`
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 60px;
  background-image: linear-gradient(rgba(0, 0, 0, 0), rgba(0, 0, 0, 1)), // 음영효과 넣기
    url(${(props) => props.bgPhoto}); // 배경으로 image를 넣는 방법
  background-size: cover; // 전체 페이지에 커버
`;
	...
	
    <Banner
     onClick={increaseIndex}
     bgPhoto={makeImagePath(data?.results[0].backdrop_path || "")} // backdrop_path가 있다면 해당 backdrop_path를 가져오고 만약에 없다면 ""빈문자열 반환
    >
     <Title>{data?.results[0].title}</Title>
     <Overview>{data?.results[0].overview}</Overview>
    </Banner>
```

=> makeImagePath를 불러오는 방법

```typescript
// utils.ts

export function makeImagePath(id: string, format?: string) {
  return `https://image.tmdb.org/t/p/${format ? format : "original"}/${id}`;
}
```



- api 로 부터 영화 목록을 json형태로 받아오는 방법

  => reactQuery를 사용해서 fetch function을 만든다.

  => useQuery를 통해 영화들과 isLoading 정보를 받아온다.

```typescript
// api.tsx

export function getMovies() { // api를 이용한 fetch function
  return fetch(`${BASE_PATH}/movie/now_playing?api_key=${API_KEY}`).then(
    (response) => response.json()
  );
}

// =======================================================================================

// Home.tsx

function Home() {
  const { data, isLoading } = useQuery<IGetMoviesResult>( // useQuery를 사용해서 정보 받기
    ["movies", "nowPlaying"],
    getMovies
  );
  const [index, setIndex] = useState(0);
  const increaseIndex = () => setIndex((prev) => prev + 1);
  return (
```



- Slider에 animation 넣기

  => `<AnimatePresence>`로 감싼다.

  => 관련 variants들을 설정한다.

```typescript
// home.tsx
	...
const Slider = styled.div`
  position: relative;
  top: -100px;	// -100px도 가능함
`;

const Row = styled(motion.div)`	// animation이 들어가는 element는 => (motion.div)
  display: grid;	// grid로 설정
  gap: 10px;	// 간격
  grid-template-columns: repeat(6, 1fr); // 1줄에 6개가 들어갈 수 있도록 설정
  position: absolute;
  width: 100%;
`;

const Box = styled(motion.div)` // animation이 들어가는 element는 => (motion.div)
  background-color: white;
  height: 200px;
  color: red;
  font-size: 66px;
`;

const rowVariants = {
  hidden: {
    x: window.outerWidth + 5, // window.outerWidth : 사용자 모니터의 가로 사이즈를 가져오는 방법
  },
  visible: {
    x: 0,
  },
  exit: {
    x: -window.outerWidth - 5,
  },
};

function Home() {
  const { data, isLoading } = useQuery<IGetMoviesResult>(
    ["movies", "nowPlaying"],
    getMovies
  );
  const [index, setIndex] = useState(0);
  const increaseIndex = () => setIndex((prev) => prev + 1);
  return (
    <Wrapper>
      {isLoading ? (
        <Loader>Loading...</Loader>
      ) : (
        <>
          <Banner
            onClick={increaseIndex}
            bgPhoto={makeImagePath(data?.results[0].backdrop_path || "")}
          >
            <Title>{data?.results[0].title}</Title>
            <Overview>{data?.results[0].overview}</Overview>
          </Banner>
          <Slider>
            <AnimatePresence>	// animation이 들어가야하는 요소들을 <AnimatePresence>로 감싸기
              <Row
                variants={rowVariants}
                initial="hidden"
                animate="visible"
                exit="exit"
                transition={{ type: "tween", duration: 1 }} // tween : 떨림없이 linear
                key={index} // react의 특성상 key값이 달라지면 새로운 element로 인식하고 리랜더링함
              >
                {[1, 2, 3, 4, 5, 6].map((i) => (
                  <Box key={i}>{i}</Box>
                ))}
              </Row>
            </AnimatePresence>
          </Slider>
        </>
	...
```

