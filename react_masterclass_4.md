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



- Silder에 영화 포스터 넣기(1)

  => 먼저, 페이지가 넘어갈 때마다 끊기는 현상 제거 

```typescript
// Home.tsx

function Home() {
  const { data, isLoading } = useQuery<IGetMoviesResult>(
    ["movies", "nowPlaying"],
    getMovies
  );
  const [index, setIndex] = useState(0);
  const [leaving, setLeaving] = useState(false); // 해당 페이지를 지나갔는지?
  const toggleLeaving = () => setLeaving((prev) => !prev); // 페이지 true / false 전환
  const increaseIndex = () => {
    if (data) {
      if (leaving) return;
      toggleLeaving();
      const totalMovies = data.results.length - 1;
      const maxIndex = Math.floor(totalMovies / offset) - 1;
      setIndex((prev) => (prev === maxIndex ? 0 : prev + 1));
    }
  };
  return (
  	...
  <AnimatePresence initial={false} onExitComplete={toggleLeaving}>
      // initial={false} : 새로고침 했을 때 animation의 initial 상태 x
      // onExitComplete={toggleLeaving} : animation의 exit가 실행될때 toggleLeaving도 실행

```



- Silder에 영화 포스터 넣기(2)

  => 위의 배경에 이미지를 넣는 방법과 비슷함

```typescript
// Home.tsx
	...
const Box = styled(motion.div)<{ bgPhoto: string }>` // (motion.div)<{ bgPhoto: string }>순서
  background-color: white; 
  background-image: url(${(props) => props.bgPhoto}); // 이미지 넣기
  background-size: cover; // 이미지 꽉 채우기
  background-position: center center; // 이미지 중앙 정렬
  height: 200px;
  font-size: 66px;
`;
	...
          <Slider>
            <AnimatePresence initial={false} onExitComplete={toggleLeaving}>
              <Row
                variants={rowVariants}
                initial="hidden"
                animate="visible"
                exit="exit"
                transition={{ type: "tween", duration: 1 }}
                key={index}
              >
                {data?.results
                  .slice(1)
                  .slice(offset * index, offset * index + offset)
                  .map((movie) => (
                    <Box
                      key={movie.id}
                      bgPhoto={makeImagePath(movie.backdrop_path, "w500")}
                    />
                  ))}
              </Row>
            </AnimatePresence>
          </Slider>
     ...
```



- Slider에 hover시 포스터의 scale를 1.3배로 보여주기

  => BoxVariants를 설정해서 motion 적용한다

  => whileHover 속성 이용

```typescript
// Home.tsx
	...
const Box = styled(motion.div)<{ bgPhoto: string }>`
  background-color: white;
  background-image: url(${(props) => props.bgPhoto});
  background-size: cover;
  background-position: center center;
  height: 200px;
  font-size: 66px;
  &:first-child {
    // Box 중 첫 번째 Box는 가운데 왼쪽을 기준으로 커짐
    transform-origin: center left;
  }
  &:last-child {
    // Box 중 마지막 번째 Box는 가운데 오른쪽을 기준으로 커짐
    transform-origin: center right;
  }
`;

const boxVariants = { // Box의 animation을 설정하는 variants
  normal: {
    scale: 1,
  },
  hover: {	// hover시 
    scale: 1.3,
    y: -50,	// 위로 50px
    transition: {	// 변화 시
      delay: 0.5,
      duration: 0.3,
      type: "tween",	// default가 spring로 되어 있기 때문에 통통 튐... => tween == linear
    },
  },
};
	...
    <Box
     key={movie.id}
     whileHover="hover" // hover 시
     initial="normal" // 기본 시작 시
     variants={boxVariants} // variants 설정
     transition={{ type: "tween" }} // boxVariants에도 둘다 설정해야함
     bgPhoto={makeImagePath(movie.backdrop_path, "w500")}
    />
	...
```



- Box가 hover 시에 영화제목이 함께 나오도록 설정

  => Box안에 info를 넣어준다.(부모가 Box이고 자식이 info이기 때문에 부모의 hover 속성을 상속받음)

  => 나머지 animation 설정들은 infoVariants에 추가 

```typescript
// Home.tsx
	...
const Info = styled(motion.div)`
  padding: 10px;
  background-color: ${(props) => props.theme.black.lighter};
  opacity: 0;	// 기본 상태에서는 보이지 않도록 opacity : 0
  position: absolute;
  width: 100%;
  bottom: 0;
  h4 {
    text-align: center;
    font-size: 18px;
  }
`;

const infoVariants = {
  hover: {
    opacity: 1,	// hover 시 보일 수 있도록 opacity : 1
    transition: {
      delay: 0.5,
      duration: 0.1,
      type: "tween",
    },
  },
};
	...
    <Box
     key={movie.id}
     whileHover="hover"
     initial="normal"
     variants={boxVariants}
     transition={{ type: "tween" }}
     bgPhoto={makeImagePath(movie.backdrop_path, "w500")}
    >
     <Info variants={infoVariants}> // info를 Box의 자식으로 설정 => whileHover 사용가능
      <h4>{movie.title}</h4>
     </Info>
    </Box>
	...
```



return 값이 있는경우 () ={}익명함수 사용

useMatch => 지금 그 url에 있는지 확인

- Slider의 특정 Box를 클릭시 해당 영화의 동영상까지 볼 수 있는 상세페이지 띄우기

  => onClick 시 useMatch로 해당 Box로 부터 movieId 받아오기

  => useMatch의 값이 있는경우(원하는 영화를 click한 경우)

  => 상세보기 창을 띄운다(이때, Box와 창의 layoutId를 movieId로 맞춘다면 자연스러운 animation연결)

```typescript
// Home.tsx
	...
function Home() {
  const history = useHistory(); // history를 통해서 라우팅 가능!!!
  const bigMovieMatch = useRouteMatch<{ movieId: string }>("/movies/:movieId");
  console.log(bigMovieMatch); // 지금 해당 url에 있는지 확인 object | null 
  const { data, isLoading } = useQuery<IGetMoviesResult>(
    ["movies", "nowPlaying"],
    getMovies
  );
  const [index, setIndex] = useState(0);
  const [leaving, setLeaving] = useState(false);
  const toggleLeaving = () => setLeaving((prev) => !prev);
  const increaseIndex = () => {
    if (data) {
      if (leaving) return;
      toggleLeaving();
      const totalMovies = data.results.length - 1;
      const maxIndex = Math.floor(totalMovies / offset) - 1;
      setIndex((prev) => (prev === maxIndex ? 0 : prev + 1));
    }
  };
  const onBoxClicked = (movieId: number) => {
    history.push(`/movies/${movieId}`); // 클릭시 해당 url로 이동한다!!!
  };
  return (
	...
                    <Box
                      layoutId={movie.id + ""} // layoutId 를 string로 맞추기
                      key={movie.id}
                      whileHover="hover"
                      initial="normal"
                      variants={boxVariants}
                      onClick={() => onBoxClicked(movie.id)} // onclick 했을 경우 return 값을 전달해야 하기 때문에 익명함수로 작성한다!!! () => {}
                      transition={{ type: "tween" }}
                      bgPhoto={makeImagePath(movie.backdrop_path, "w500")}
                    >
                      <Info variants={infoVariants}>
                        <h4>{movie.title}</h4>
                      </Info>
                    </Box>
                  ))}
              </Row>
            </AnimatePresence>
          </Slider>
          <AnimatePresence>
            {bigMovieMatch ? (
              <motion.div
                layoutId={bigMovieMatch.params.movieId} // layoutId 맞추기
                style={{
                  position: "absolute",
                  width: "40vw",
                  height: "80vh",
                  backgroundColor: "red",
                  top: 50,
                  left: 0,
                  right: 0,
                  margin: "0 auto",
                }}
              />
            ) : null}
          </AnimatePresence>
	...
```

- useHistory() 사용하기

  => JSX에서는 사용할 수 없음(Link와의 차이점)

```typescript
const history = useHistory(); 

const onBoxClicked = (movieId: number) => {
 history.push(`/movies/${movieId}`); // 클릭시 해당 url로 이동한다!!!
};
```

- number을 string으로 만드는 간단한 방법

```typescript
layoutId={movie.id + ""}
```

- string을 number으로 만드는 간단한 방법

```typescript
layoutId={+movie.id}
```



`<></>`안에는 서로 다른 component를 동시에 보여줄때 사용 

scroll position => useViewportScroll() => 사용자의 화면 scroll상태를 확인할 수 있음

- 상세보기 창을 scroll 상태와 상관없이 화면의 중앙에 나타나게 함 + 창 밖을 누르면 원래 상태로 복귀

  => useViewportScroll()을 사용해서 scrollY 값을 가져오고 값을 상세보기 창의 top속성에 적용

  => Overlay component를 onClick했을 경우 history.push로 다른 url로 보내기

```typescript
// Home.tsx

	...
const Overlay = styled(motion.div)`
  position: fixed; // fixed 설정을 추가해야 Overlay가 페이지 전체에 적용
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  opacity: 0;
`;

const BigMovie = styled(motion.div)` // top: 은 직접 style={}에 설정
  position: absolute;
  width: 40vw;
  height: 80vh;
  left: 0;
  right: 0;
  margin: 0 auto;
`;
	...
function Home() {
  const history = useHistory();
  const bigMovieMatch = useRouteMatch<{ movieId: string }>("/movies/:movieId");
  const { scrollY } = useViewportScroll(); // scroll 값 가져오기
	...
  const onOverlayClick = () => history.push("/"); // Overlay를 click시 url을 라우팅 
	...
              <>	// <></>안에서는 서로 다른 component를 여러개 랜더링할 수 있음
                <Overlay
                  onClick={onOverlayClick}
                  exit={{ opacity: 0 }}
                  animate={{ opacity: 1 }}
                />
                <BigMovie
                  style={{ top: scrollY.get() + 100 }} // scrollY.get()+100 : 사용자가 scroll한 Y값을 .get()로 가져오고 100px만큼 내린다.
                  layoutId={bigMovieMatch.params.movieId}
                >
                  hello
                </BigMovie>
              </>
      ...
```



- 선택한 Box의 상세보기 창에 영화포스터, 제목, 줄거리 넣기

  => useMatch의 값이 존재한다면 

  => api로 넘어온 data에서 find()를 하고 

  => data?.results.find((movie) => movie.id === +bigMovieMatch.params.movieId);

  => 의 결과를 clickedMovie 에 담는다.

  => 그러면 clickedMovie 안에 영화에 대한 정보들이 담긴다.

```typescript
// Home.tsx

	...
const BigCover = styled.div`
  width: 100%;
  background-size: cover;
  background-position: center center;
  height: 400px;
`;

const BigTitle = styled.h3`
  color: ${(props) => props.theme.white.lighter};
  padding: 20px;
  font-size: 46px;
  position: relative;
  top: -80px;
`;

const BigOverview = styled.p`
  padding: 20px;
  position: relative;
  top: -80px;
  color: ${(props) => props.theme.white.lighter};
`;
	...
  const clickedMovie = // clickedMovie 안에 선택한 영화정보를 담기
    bigMovieMatch?.params.movieId &&
    data?.results.find((movie) => movie.id === +bigMovieMatch.params.movieId);
	...
                  {clickedMovie && ( // clickedMovie && : clickedMovie가 있다면 <></>를 랜더링
                    <>
                      <BigCover
                        style={{
                          backgroundImage: `linear-gradient(to top, black, transparent), url(${makeImagePath(
                            clickedMovie.backdrop_path,
                            "w500"
                          )})`,
                        }}
                      />
                      <BigTitle>{clickedMovie.title}</BigTitle>
                      <BigOverview>{clickedMovie.overview}</BigOverview>
                    </>
                  )}
     ...
```



useForm() => form을 이용할 때 유효성 검사bb

useHistory() => 상용자가 어떠한 component를 클릭하지 않고서도 다른 url로 보낼 수 있음

useLocation() => url로부터 넘어온 정보를 확인할 수 있음

URLSearchParams() => url의 key=value에서 value값만 가져올 수 있음

=> useLocation()으로 부터 value를 파싱하는 것은 꽤나 까다롭기 때문에

=> URLSearchParams()을 사용

- Search 페이지

  => useForm()을 통해서 사용자의 입력내용을 받아옴

  => onSubmit={handleSubmit(onValid)}을 통해 유효성 검사를 진행

  => onValid안에서 history.push(`/search?keyword=${data.keyword}`);로 라우팅

  => useLocation()으로 url데이터를 받고 URLSearchParams로 파싱

```typescript
// Header.tsx

	...
  const history = useHistory();
  const { register, handleSubmit } = useForm<IForm>();
  const onValid = (data: IForm) => {
    history.push(`/search?keyword=${data.keyword}`); // url 라우팅
  };
	...
<Col>
        <Search onSubmit={handleSubmit(onValid)}> // onSubmit으로 검사
          <motion.svg
            onClick={toggleSearch}
            animate={{ x: searchOpen ? -185 : 0 }}
            transition={{ type: "linear" }}
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
            {...register("keyword", { required: true, minLength: 2 })} // useForm 사용 ...register ~
            animate={inputAnimation}
            initial={{ scaleX: 0 }}
            transition={{ type: "linear" }}
            placeholder="Search for movie or tv show..."
          />
        </Search>
      </Col>
  	...
```

```typescript
// Search.tsx

import { useLocation } from "react-router";

function Search() {
  const location = useLocation(); // useLocation()로 url정보 받아오기
  const keyword = new URLSearchParams(location.search).get("keyword"); // new URLSearchParams(location.search).get("keyword")으로 keyword의 value 값 파싱
  console.log(keyword);
  return null;
}
export default Search;
```

끝....
