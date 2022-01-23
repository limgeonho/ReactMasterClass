npx create-react-app react-masterclass

npm i styled-components

```react
function App() {
  return (
    <div style={{ display: "flex"}}>
      <div style={{ backgroundColor: "teal", width: 100, height: 100}}		    </div>
      <div style={{ backgroundColor: "tomato", width: 100, height: 100}}>		 </div>
    </div>
  );
}
```

=>

```react
import styled from "styled-components";

// styled.(html태그)`(css)` => ``(백틱)
const Father = styled.div`
  display: flex
;`;

const BoxOne = styled.div`
  background-color: teal;
  width: 100px;
  height: 100px;
`;

const BoxTwo = styled.div`
  background-color: tomato;
  width: 100px;
  height: 100px;
`;

const Text = styled.span`
  color: white;
`;

function App() {
  return (
    <Father>
      <BoxOne>
        <Text>Hello</Text>
      </BoxOne>
      <BoxTwo />
    </Father>
  );
}

export default App;
// styled-components를 사용하면 더 가독성있고 간결하게 코드를 작성할 수 있다.
```

=> 중복제거 ${props}이용 + const Circle=> styled(Box)``확장

```react
import styled from "styled-components";

// styled.(html태그)`(css)` => ``(백틱)
const Father = styled.div`
  display: flex
;`;

const Box = styled.div`
  background-color: ${(props) => props.bgColor}; // 중복 제거 {props를 통해}-동적으로 작용
  width: 100px;
  height: 100px;
`;

const Circle = styled(Box)` // styled(Box)''; => Box의 특성을 전부 가지고 + 추가적으로 Circle만의 특성도 추가함
  border-radius: 50px;
`;

function App() {
  return (
    <Father>
      <Box bgColor="teal"/>
      <Circle bgColor="tomato"/>
    </Father>
  );
}

export default App;
```

=>

```react
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

const Btn = styled.button`
  color: white;
  background-color: tomato;
  border: 0;
  border-radius: 15px
`;

function App() {
  return (
    <Father>
      <Btn>Login</Btn>
      <Btn as="a" href="/">Login</Btn> // Btn의 속성은 사용하면서 html태그만 바꾸고 싶을때 as="(원하는 html 태그)"
    </Father>
  );
}

export default App;
```

=>

```react
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

const Input = styled.input.attrs({required:true})` // .attrs안에 객체 형태로 원하는 설정을 넣을 수 있음
  background-color: tomato;
`;

function App() {
  return (
    <Father>
      <Input/>
      <Input/>
      <Input/>
      <Input/>
      <Input/>
    </Father>
  );
}

export default App;
```

=> animation

```react
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
`;

const rotatinAnimation = keyframes`
  0% {
    transform:rotate(0deg);
    border-radius:0px;
  }
  50% {
    border-radius:100px;
  }
  100%{
    transform:rotate(360deg);
    border-radius:0px;
  }
`;

const Box = styled.div`
  background-color: tomato;
  height: 200px;
  width: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotatinAnimation} 1s linear infinite;	//animation을`${}`
  span {					// Box안에 span태그의 설정을 따로 넣을 수 있음
    font-size: 36px;
    &:hover {				// &:hover == span:hover과 같은 역할
      font-size: 48px;
    }
    &:active {				// &:active == span:active
      opacity: 0;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
      	<span>🤩</span>
      </Box>
    </Wrapper>
  );
}

export default App;

```

=> 커스터 마이징 component

```react
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
`;

const rotatoinAnimation = keyframes`
  0% {
    transform:rotate(0deg);
    border-radius:0px;
  }
  50% {
    border-radius:100px;
  }
  100%{
    transform:rotate(360deg);
    border-radius:0px;
  }
`;

const Emoji = styled.span`	// 커스터마이징한 component
  font-size: 36px;
`;

const Box = styled.div`
  background-color: tomato;
  height: 200px;
  width: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotatoinAnimation} 1s linear infinite;
  ${Emoji}:hover {		// ${Emoji}를 통해 커스터마이징한 component사용가능
      font-size: 98px;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <Emoji>🤩</Emoji>	//Emoji의 기능은 Box안에 있는 조건에만 작동함
      </Box>
      <Emoji>🔥</Emoji>
    </Wrapper>
  );
}

export default App;
```

=> Theme(dark/light mode)

```react
// index.html
import React from "react";
import ReactDOM from "react-dom";
import { ThemeProvider } from "styled-components";
import App from "./App";

// darkmode, lightmode를 하기 위해서는 darkTheme와 lightTheme가 같은 css요소들을 가지고 있어야한다.
const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
};

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
};

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}> 
      <App />
    </ThemeProvider>	// ThemeProvider 하위에 있는 모든 component들은 ThemeProvider의 props에 접근할 수 있다.
  </React.StrictMode>,
  document.getElementById("root")
);

// App,js
import styled from "styled-components";

const Title = styled.h1`
  color: ${(props) =>
    props.theme
      .textColor}; // color은 Title만의 고유한 속성이 아니라 Title > App > ThemeProvider 로 부터 온다.
`;

const Wrapper = styled.div`
  display: flex;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
  background-color: ${(props) => props.theme.backgroundColor};
`;

function App() {
  return (
    <Wrapper>
      <Title>Hello</Title>
    </Wrapper>
  );
}

export default App;
```



=============================================================================

Typescript

- 브라우저는 Typescript를 알아듣지 못함

- Typescript를 알아서 javascript로 변환해 줌

- 실행전에 문법적인 오류들을 찾아낼 수 있음

- ```typescript
  const plus = (a:number, b:number) => a+b;
  ```

- 타입을 지정할 수 없는 Javascript에 타입을 지정해줄 수 있는 언어 == Typescript

npx create-react-app my-typescript-app --template typescript

npm i --save-dev @types/styled-components

```typescript
// App.tsx
import Circle from "./Circle";

function App() {
  return (
    <div>
      <Circle bgColor="teal" />
      <Circle bgColor="toamto" />
    </div>
  );
}

export default App;


// index.tsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);

// Cicle.tsx
import styled from "styled-components";

const Container = styled.div<CircleProps>`
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
`;

interface CircleProps {
  // props를 검증해주고 보호해줌 interface
  // PropTypes 대신 사용함 => props의 type를 미리 검증할 수 있음
  bgColor: string; // Circle({ bgColor }:  CircleProps)의 bgColor과 이름이 같아야함
}

function Circle({ bgColor }: CircleProps) {
  return <Container bgColor={bgColor} />;
}

export default Circle;

```

=> required 하지 않은 props를 typescript로 받는 방법

```typescript
// App.tsx
import Circle from "./Circle";

function App() {
  return (
    <div>
      <Circle borderColor="yellow" bgColor="teal" />
      <Circle text="im here" bgColor="tomato" />
    </div>
  );
}

export default App;

// Circle.tsx
import styled from "styled-components";

interface ContainerProps {
  bgColor: string;
  borderColor: string;
}

const Container = styled.div<ContainerProps>`
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
  border: 1px solid ${(props) => props.borderColor};
`;

interface CircleProps {
  // props를 검증해주고 보호해줌 interface
  // PropTypes 대신 사용함 => props의 type를 미리 검증할 수 있음
  bgColor: string; // Circle({ bgColor }:  CircleProps)의 bgColor과 이름이 같아야함
  borderColor?: string; // ?를 붙이면 optional 한 props로 바뀐다
  text?: string;
}

function Circle({ bgColor, borderColor, text = "default text" }: CircleProps) { // text = "default text" 는 default 값 설정방법
  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}> // borderColor={borderColor ?? bgColor} 는 optional 있으면 borderColor 없으면 bgColor
      {text}
    </Container>
  );
}

export default Circle;
```

=> State

```typescript
import { useState } from "react";
import styled from "styled-components";

interface ContainerProps {
  bgColor: string;
  borderColor: string;
}

const Container = styled.div<ContainerProps>`
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
  border: 1px solid ${(props) => props.borderColor};
`;

interface CircleProps {
  // props를 검증해주고 보호해줌 interface
  // PropTypes 대신 사용함 => props의 type를 미리 검증할 수 있음
  bgColor: string; // Circle({ bgColor }:  CircleProps)의 bgColor과 이름이 같아야함
  borderColor?: string; // ?를 붙이면 optional 한 props로 바뀐다
  text?: string;
}

function Circle({ bgColor, borderColor, text = "default text" }: CircleProps) {
  // text = "default text" 는 default 값 설정방법
  const [value, setValue] = useState<number | string>(1); // useState<number|string>(1) => <>안에 있는 type로 protect 할 수 있음
  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>
      {text}
    </Container>
  );
}

export default Circle;
```

=> Forms

```typescript
import React, { useState } from "react";

function App() {
  const [value, setValue] = useState("");
  const onChange = (event: React.FormEvent<HTMLInputElement>) => { // (event: React.FormEvent<HTMLInputElement>) => event가 input에서 넘어오는 것을 protect 할 수 있음
    const {
      currentTarget: { value },
    } = event;
    setValue(value); // 앞에서 본거와 같이 useState에서 typescript가 알아서 value의 속성이 string 인 것을 알기 때문에 protect 할 수 있음 
  };

  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("hello", value);
  };

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          onChange={onChange}
          value={value}
          type="text"
          placeholder="username"
        />
        <button>Login</button>
      </form>
    </div>
  );
}

export default App;
```

=> Themes (기존의 theme과 형태는 같지만 ts를 사용해서 theme이 가지고 있는 요소를 자동완성과 protect 받을 수 있다.)

```typescript
// App.tsx
import styled from "styled-components";

const Container = styled.div`
  background-color: ${(props) => props.theme.bgColor}; // ${(props) => props.theme.bgColor} 에서 ts가 알아서 theme의 요소들을 보여준다. => 오타나 누락할 일 protect
`;

const H1 = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

function App() {
  return (
    <Container>
      <H1>Protected</H1>
    </Container>
  );
}

export default App;

// index.tsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { ThemeProvider } from "styled-components";
import { darkTheme, lightTheme } from "./theme";

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}> // ThemeProvider 를 가져오고 기존의 theme와 같다.
      <App />
    </ThemeProvider>
    r
  </React.StrictMode>,
  document.getElementById("root")
);

//theme.ts
import { DefaultTheme } from "styled-components";

// 해당 theme.ts 파일에서는 DefaultTheme 를 통해 lightTheme 과 darkTheme 의 기본적인 틀을 보관하고 protect 받을 수 있는 default를 설정한다.
export const lightTheme: DefaultTheme = {
  bgColor: "white",
  textColor: "black",
  btnColor: "tomato",
};

export const darkTheme: DefaultTheme = {
  bgColor: "black",
  textColor: "white",
  btnColor: "teal",
};

```

참고 : 만약에 React에서 사용하는 라이브러리가 ts에 없다면

```shell
npm i --save-dev @types/(라이브러리) # 시도하기
```



=============================================================================

CRYPTO TRACKER





router에서 `<Switch>`를 사용하기 위해서는 react-router-dom의 버전을 5버전으로 설치해야함

```shell
npm i react-router-dom react-query
npm uni react-router-dom 
npm i react-router-dom@5.3.0
npm i --save-dev @types/react-router-dom
```



배경색 추천 사이트

https://flatuicolors.com/palette/gb

```css
// css 참고사항
a {
  text-decoration:none;
  color:inherit; // a태그가 선택되었을 때도 글자색이 변하지 않도록하기 위한 조건 
}
```



map함수 활용

```typescript
// Coins.tsx
import { Link } from "react-router-dom";
import styled from "styled-components";

const Container = styled.div`
  padding: 0px, 20px;
`;

const Header = styled.header`
  height: 15vh;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Title = styled.h1`
  font-size: 48px;
  color: ${(props) => props.theme.accentColor};
`;

const CoinsList = styled.ul``;

const Coin = styled.ul`
  background-color: white;
  color: ${(props) => props.theme.bgColor};
  border-radius: 15px;
  margin-bottom: 10px;
  a {
    padding: 20px;
    transition: color 0.2s ease-in;
    display: block;
  }
  &:hover {
    a {
      color: ${(props) => props.theme.accentColor};
    }
  }
`;

const coins = [
  {
    id: "btc-bitcoin",
    name: "Bitcoin",
    symbol: "BTC",
    rank: 1,
    is_new: false,
    is_active: true,
    type: "coin",
  },
];

function Coins() {	// styled-component를 사용하면 아래처럼 태그의 이름만 보고 파악가능
  return (
    <Container>
      <Header>
        <Title>코인</Title>
      </Header>
      <CoinsList>
        {coins.map((coin) => (
          <Coin key={coin.id}>
            <Link to={`/${coin.id}`}>{coin.name} &rarr;</Link>
          </Coin>
        ))}
      </CoinsList>
    </Container>
  );
}

export default Coins;
```



async await

```typescript
  const [coins, setCoins] = useState<CoinInterface[]>([]);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    (async () => {  // => {()();} 하면 첫 번째 나오는 ()안에 있는 함수는 바로 실행된다
      const response = await fetch("https://api.coinpaprika.com/v1/coins");
      const json = await response.json(); // api로부터 가져온 response를 json형태로 바꾼다
      setCoins(json.slice(0, 100)); // 바꾼 json을 100개까지만 가져오도록 slice하고 setCoins에 넣는다
      setLoading(false);
    })();
  }, []);
```



react-router-dom 6버전 이후 부터는 제네릭을 지원하지 않음

```typescript
react-router-dom v6 부터 제네릭을 지원하지 않습니다.

interface RouterState {
name: string;
}

const location = useLocation();
const name = location.state as RouterState;
```



`<Link>`를 통해서 URL안에 parameter 속에 path뿐 만 아니라 state로 보내는 방법

```typescript
<Link				// to={{ key: value}}
	to={{
      pathname: `/${coin.id}`,
      state: { name: coin.name },
    }}
></Link>
```



useLocation => 위에서 URL안에 들어있던 state를 가져오는 방법

```typescript
interface RouteState {
  name: string;
}

function Coin() {
  const [loading, setLoading] = useState(true);
  const { coinId } = useParams<RouteParams>();
  const { state } = useLocation<RouteState>(); // useLocation + state에 대한 props
  return (
    <Container>
      <Header>
        <Title>{state?.name || "Loading..."}</Title> // state?.name => name없으면 loading...
      </Header>
      {loading ? <Loader>Loading...</Loader> : null}
    </Container>
  );
}
```



Nested Route

=> 페이지 내부에서 탭을 선택하면 해당 탭이 열리는 페이지를 보여주고 싶을때

=> 탭을 선택할 때마다 기존의 url에서 해당 탭 이름으로 추가된다.

=> price 탭 선택시 : bitcoin/price 

=> chart 탭 선택시 : bitcoin/chart

=> `<Switch>` -> `<Route>` -> `<Price>` 순으로 추가 작성

```typescript
// Coin.tsx
fuction Coin (){
    ...
    return(
    	<Switch>
            <Route path={`/${coinId}/price`}> // path={`/${coinId}/price`} 해당 url까지 들어오고 바뀌는 부분은 coinId니까 useLocation에서 받아온 후 뒤에 /price로 분기
              <Price />
            </Route>
            <Route path={`/${coinId}/chart`}>
              <Chart />
            </Route>
         </Switch>
    );
}
```



동적 url

```typescript
// 둘 다 가능
<Route path={`/${coinId}/price`}>

<Route path={`/:coinId/price`}>
```



useRouteMatch

```typescript
const priceMatch = useRouteMatch("/:coinId/price"); // 해당 url 이 "/:coinId/price" 와 일치하면 Object 아니면 null 반환
const chartMatch = useRouteMatch("/:coinId/chart");
```



props의 타입 검사하는 법

```react
const Tab = styled.span<{ isActive: boolean }>``; 
// span<{ isActive: boolean }> 의 props의 타입을 검사

<Tab isActive={chartMatch !== null}> // 해당chartMatch의 결과에 따라 props로 isActive
	<Link to={`/${coinId}/chart`}>Chart</Link>
</Tab>
```



=============================================================================

React Query

npm i react-query



fetcher function

기존에 있던 async await fetch 함수를 => fetcher function으로 뺀다.

```typescript
// api.tsx
export function fetchCoins() {
  return fetch("https://api.coinpaprika.com/v1/coins").then((response) =>
    response.json()
  );
}
```



useQuery

=> useQuery 는 2가지 인자를 받는다(key값, fetcher function)

=> useQuery 의 반환 값은 가지고 있는 fetcher function 이 loading 인지 여부와 loading이 끝났다면 fetcher function의 return data를 반환해준다. + 캐싱도 도와줌 (useState역할)

```react
// Coins.tsx
import { useQuery } from "react-query";

function Coins() {
  /*
  const [coins, setCoins] = useState<ICoin[]>([]);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    (async () => {
      // => {()();} 하면 첫 번째 나오는 ()안에 있는 함수는 바로 실행된다
      const response = await fetch("https://api.coinpaprika.com/v1/coins");
      const json = await response.json(); // api로부터 가져온 response를 json형태로 바꾼다
      setCoins(json.slice(0, 100)); // 바꾼 json을 100개까지만 가져오도록 slice하고 setCoins에 넣는다
      setLoading(false);
    })();
  }, []);
  */
  const { isLoading, data } = useQuery<ICoin[]>("allCoins", fetchCoins);  // 위의 주석들을 한 줄로 대체

  // styled-component를 사용하면 아래처럼 태그의 이름만 보고 파악가능
  return (
    <Container>
      <Header>
        <Title>코인</Title>
      </Header>
      {isLoading ? (
        <Loader>Loading...</Loader>
      ) : (
        <CoinsList>
          {data?.slice(0, 100).map((coin) => ( // data가 있다면 slice해서 100개만 보여
            <Coin key={coin.id}>
              <Link
                to={{
                  pathname: `/${coin.id}`,
                  state: { name: coin.name },
                }}
              >
                <Img
                  src={`https://cryptoicon-api.vercel.app/api/icon/${coin.symbol.toLowerCase()}`}
                />
                {coin.name} &rarr;
              </Link>
            </Coin>
          ))}
        </CoinsList>
      )}
    </Container>
  );
}

export default Coins;
```



ReactQueryDevtools

=> 개발자 도구에서 react-query의 캐시 등 여러가지 내용을 추가로 보여줌

```react
import { ReactQueryDevtools } from "react-query/devtools";

function App() {
  return (
    <>
      <GlobalStyle />
      <Router />
      <ReactQueryDevtools initialIsOpen={true} />
    </>
  );
}
```



Coin.tsx에 있던 async와 await 그리고 useState를 => useQuery로 바꿔보자

```react
// api.tsx

const BASE_URL = `https://api.coinpaprika.com/v1`;

export function fetchCoins() {
  return fetch(`${BASE_URL}/coins`).then((response) => response.json());
}

export function fetchCoinInfo(coinId: string) {
  return fetch(`${BASE_URL}/coins/${coinId}`).then((response) =>
    response.json()
  );
}

export function fetchCoinTickers(coinId: string) {
  return fetch(`${BASE_URL}/tickers/${coinId}`).then((response) =>
    response.json()
  );
}
```

before

```react
const { coinId } = useParams<RouteParams>();
const { state } = useLocation<RouteState>();
const priceMatch = useRouteMatch("/:coinId/price"); // 해당 url 이 "/:coinId/price" 와 일치하면 Object 아니면 null 반환
const chartMatch = useRouteMatch("/:coinId/chart");
const [loading, setLoading] = useState(true);
const [info, setInfo] = useState<InfoData>();
const [priceInfo, setPriceInfo] = useState<PriceData>();

function Coin() {
  useEffect(() => {
    (async () => {
      const infoData = await (
        await fetch(`https://api.coinpaprika.com/v1/coins/${coinId}`)
      ).json();
      const priceData = await (
        await fetch(`https://api.coinpaprika.com/v1/tickers/${coinId}`)
      ).json();
      setInfo(infoData);
      setPriceInfo(priceData);
      setLoading(false);
    })();
  }, [coinId]);
```

after

```react
function Coin() {
  const { coinId } = useParams<RouteParams>();
  const { state } = useLocation<RouteState>();
  const priceMatch = useRouteMatch("/:coinId/price"); // 해당 url 이 "/:coinId/price" 와 일치하면 Object 아니면 null 반환
  const chartMatch = useRouteMatch("/:coinId/chart");
  
  const { isLoading: infoLoading, data: infoData } = useQuery<InfoData>(
    ["info", coinId],
    () => fetchCoinInfo(coinId)
  );
  const { isLoading: tickersLoading, data: tickersData } = useQuery<PriceData>(
    ["tickers", coinId],
    () => fetchCoinTickers(coinId)
  );
  // isLoading: infoLoading, data: infoData는 아래 useQuery가 하나 더 생기기 때문에 구별해 주기 위해서 각각의 별칭을 넣은 것임
  // () => fetchCoinInfo(coinId) 는 fetch function안에 파라미터가 들어가기 때문에 화살표 함수로 작성
  const loading = infoLoading || tickersLoading;
```



useQuery의 장점

1. 해당 함수가 isLoading되었는지에 대한 여부를 알려줌(fetch function을 작성해서 연결해줌)
2. fetch function을 통해서 전달받은 데이터를 캐싱해서 가지고 있음 => api 다시 호출할 필요가 적어짐



=============================================================================

Chart

```react
// api.tsx
export function fetchCoinHistory(coinId: string) {
  const endDate = Math.floor(Date.now() / 1000);
  const startDate = endDate - 60 * 60 * 24 * 7 * 2;
  return fetch(
    `${BASE_URL}/coins/${coinId}/ohlcv/historical?start=${startDate}&end=${endDate}`
  ).then((response) => response.json());
}

// chart.tsx
import { useQuery } from "react-query";
import { fetchCoinHistory } from "../api";

interface ChartProps {
  coinId: string;
}

interface IHistorical {
  time_open: string;
  time_close: string;
  open: number;
  high: number;
  low: number;
  close: number;
  volume: number;
  market_cap: number;
}

function Chart({ coinId }: ChartProps) {  // coinId를 전달받고 ChartProps로 검증
  const {} = useQuery<IHistorical[]>(["ohlcv", coinId], () => fetchCoinHistory(coinId));
  return <h1>Chart</h1>;
}

// <IHistorical[]> 에서 []하는 이유는 data 한 세트가 <IHistorical>이고 이러한 세트를 2주치 총 14개를 받아와야하기 때문에 []로 처리해야해서 <IHistorical[]>로 사용한다.

export default Chart;
```



JS data 시각화 library

```shell
npm install --save react-apexcharts apexcharts
```

나머지는 찾아서 원하는대로 커스터마이징하면 끝



useQuery의 세 번째 argument 사용법

=> 세 번째 인자는 해당 fetch fuction을 얼마 주기로 다시 실행하는지 시간을 밀리세컨으로 정할 수 있음

useQuery`<type를 정하는 interface>`("key" || ["key", props], fetch func || () => fetch func(props), { refetchInterval: 10000, } 

```react
const { isLoading, data } = useQuery<IHistorical[]>(
    ["ohlcv", coinId],
    () => fetchCoinHistory(coinId),
    {
      refetchInterval: 10000,	// 10 초마다 다시 불러오기
    }
  );
```



React helmet

=> `<title>`에 페이지 마다 원하는 내용을 작성할 수 있게 도와준다

=> favicon도 작성가능하고 css 스타일도 적용가능

```shell
npm i react-helmet
npm i --save-dev @types/react-helmet
```

```react
// Coins.tsx
return (
    <Container>
      <Helmet>	// react-helmet를 사용해서 `<title>`에 적용
        <title>코인</title>
      </Helmet>
      ...
      
```

