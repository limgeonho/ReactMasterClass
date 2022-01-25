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



=============================================================================



Recoil => state management library

Recoil은 상태관리를 도와주는 라이브러리이고 아직은 recoil을 사용하지 않고 상태를 관리해보자

darkmode, lightmode => 토글버튼을 만들어서 모드 변경

```react
// App.js
function App() {
  const [isDark, setIsDark] = useState(true);
  const toggleDark = () => setIsDark((current) => !current); // useState의 setIsDark의 첫 번째 인자는 기존에 가지고 있던 isDark 값임!!
  return (
    <>
      <ThemeProvider theme={isDark ? darkTheme : lightTheme}>
        <button onClick={toggleDark}>Toggle Mode</button>
        <GlobalStyle />
        <Router />
        <ReactQueryDevtools initialIsOpen={true} />
      </ThemeProvider>
    </>
  );
}

export default App;
```

=> global state가 필요한 이유

코드는 따로 적지 않겠음...

하지만 state가 없고 props로만 데이터의 상태를 확인할 수 있다면 코드들이 매우 복잡해진다

=> props는 부모와 자식사이에서만 전달이 가능하기 때문에 멀리있는 component에서 해당 data의 state를 접근하고 싶을 때는 한 단계씩 내려줘야하는 불편함이 있다

=> 따라서, 이를 해결하고자 state management가 나타남 => recoil

Recoil에서는 Atom이라는 공간안에 global하게 저장하고 싶은 것을 넣어놓고 사용한다

=> 필요한 component가 직접 중간 전달과정을 거치지 않고 Atom에 접근해서 해당 value를 가져올 수 있다.



recoil 설치방법

```shell
npm install recoil
```



atom에 원하는 값의 상태를 보관하는 방법 => 꺼내오는 방법(useRecoilValue)

```react
// atoms.ts
import { atom } from "recoil";

// atom을 만들고 내부적으로는 key: value 형태로 적어준다.
export const isDarkAtom = atom({
  key: "isDark",
  default: false,
});

// index.tsx => 먼저, <RecoilRoot>으로 감싼다.
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { QueryClient, QueryClientProvider } from "react-query";
import { RecoilRoot } from "recoil";

const queryClient = new QueryClient();

ReactDOM.render(
  <React.StrictMode>
    <RecoilRoot>
      <QueryClientProvider client={queryClient}>
        <App />
      </QueryClientProvider>
    </RecoilRoot>
  </React.StrictMode>,
  document.getElementById("root")
);

// App.tsx
function App() {
  // recoil 사용으로 인해 불필요해짐
  // const [isDark, setIsDark] = useState(true);
  // const toggleDark = () => setIsDark((current) => !current); // useState의 setIsDark의 첫 번째 인자는 기존에 가지고 있던 isDark 값임!!
  const isDark = useRecoilValue(isDarkAtom);
  return (
    <>
      <ThemeProvider theme={isDark ? darkTheme : lightTheme}>
        <GlobalStyle />
        <Router />
        <ReactQueryDevtools initialIsOpen={true} />
      </ThemeProvider>
    </>
  );
}

export default App;
```

```react
// atom에서 가져오기
const isDark = useRecoilValue(isDarkAtom);
```



=> atom에서 가져온 value를 수정하는 방법(useSetRecoilState) useState와 비슷하게 동작함(첫 번째 인자가 현재 값)

```react
// Coins.tsx
function Coins() {
  const setDarkAtom = useSetRecoilState(isDarkAtom);
  const toggleDarkAtom = () => setDarkAtom((prev) => !prev); // setDarkAtom는 useState와 비슷하게 동작함!
  const { isLoading, data } = useQuery<ICoin[]>("allCoins", fetchCoins); // 위의 주석들을 한 줄로 대체

  // styled-component를 사용하면 아래처럼 태그의 이름만 보고 파악가능
  return (
    <Container>
      <Helmet>
        <title>코인</title>
      </Helmet>
      <Header>
        <Title>코인</Title>
        <button onClick={toggleDarkAtom}>Toggle Mode</button>
      </Header>
          ...
```

```react
const setDarkAtom = useSetRecoilState(isDarkAtom);
// useSetRecoilState는 isDarkAtom의 key인 isDark를 수정할 수 있는 function을 반환한다.
```

결론: recoil 상태관리에 매우 편함...bb



==============================================================================



Todo with Recoil



기존의 useState를 이용해서 todo작성

```react
// ToDoList.tsx
import React, { useState } from "react";

function ToDoList() {
  const [toDo, setToDo] = useState("");
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setToDo(value);
  };
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log(toDo);
    console.log(event);
  };
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input onChange={onChange} value={toDo} placeholder="Write a to do" />
        <button>Add</button>
      </form>
    </div>
  );
}

export default ToDoList;
```



react hook form

react에서 form을 관리하기 매우 편한 라이브러리

```shell
npm install react-hook-form
```

react-hook-form 을 사용하는 방법 => useForm()

위의 onChange, onSubmit, useState들은 결국 하나의 input을 처리하기 위해 존재하는 것들이었다.

하지만 useForm을 사용해서 위의 복잡한 코드들을 한 줄로 줄일 수 있다.

```react
function ToDoList() {
  const { register, watch } = useForm();
  console.log(watch());
  return (
    <div>
      <form>
        <input {...register("email")} placeholder="Email" /> // {...register("")}로끝
        <input {...register("firstName")} placeholder="First Name" />
        <input {...register("lastName")} placeholder="Last Name" />
        <input {...register("username")} placeholder="Username" />
        <input {...register("password")} placeholder="Password" />
        <input {...register("password1")} placeholder="Password1" />
        <button>Add</button>
      </form>
    </div>
  );
}
export default ToDoList;
```

```react
// 기존의 onChange를 대체하는 방법 register, watch

const { register, watch } = useForm();
// useForm은 기본적으로 register와 watch를 가진다.
// register로 등록한 value는 알아서 state가 관리되고 
// watch는 해당 value가 변화할 때마다 감지한다.

`<input {...register("emial") placeholder="Email" /}>` 방식으로 {...register("value")}
```



useForm() 의 input 값의 에러처리를 위한 인자 => handleSubmit, formState

handleSubmit, formState를 이용하면 회원가입기능 같은 form에서 좀 더 빠르고 쉽게 error처리를 할 수 있다.(추천기능!!)

```react
function ToDoList() {
  const { register, handleSubmit, formState } = useForm();
  const onValid = (data: any) => {
    //onSubmit={handleSubmit(onValid)} 를 위해서 반드시 필요한 인자
    console.log(data);
  };
  console.log(formState.errors); // input 값에 에러가 정해놓은 에러가 있을 경우에 메세지를 볼 수 있음
  return (
    <div>
      <form
        style={{ display: "flex", flexDirection: "column" }}
        onSubmit={handleSubmit(onValid)}
      >
        <input {...register("email", { required: true })} placeholder="Email" />
        <input
          {...register("firstName", { required: true })}
          placeholder="First Name"
        />
        <input
          {...register("lastName", { required: true })}
          placeholder="Last Name"
        />
        <input
          {...register("username", { required: true, minLength: 10 })}
          placeholder="Username"
        />
        <input
          {...register("password", { required: true, minLength: 5 })}
          placeholder="Password"
        />
        <input
          {...register("password1", {
            required: "Password is required",
            minLength: {
              value: 5,
              message: "Your password is too short.",
            },
          })}
          placeholder="Password1"
        />
        <button>Add</button>
      </form>
    </div>
  );
}
export default ToDoList;
```



onSubmit={handleSubmit(onValid)}

```react
// handleSubmit(onValid) 은 반드시 함수로 만들어진 onValid를 인자로 갖는다.
const onValid = (data: any) => {
    //onSubmit={handleSubmit(onValid)} 를 위해서 반드시 필요한 인자
    console.log(data);
}; // onValid는 form이 submit되기 전에 받은 input들을 점검하고 이상이 없을 시에 submit를 완료한다. 

<form onSubmit={handleSubmit(onValid)}>
```



...register("value", {required: true. minLength: 5})

```react
// ...register안에 input의 Validation을 넣는 방법 => { required: true, minLength: 5 } 으로 넣고 통과하지 못할 시에 해당 사유가 출력된다.
<input {...register("password", { required: true, minLength: 5 })} placeholder="Password"/>
```

```react
<input {...register("password1", {required: "Password is required",
	minLength: {
		value: 5,
		message: "Your password is too short.",
		},
	})}
	placeholder="Password1"/>
// minLength는 5이고 충족하지 못했을 경우 message에 "Your password is too short."가 나옴
```



=> 위에서 message에 출력한 문자를 사용자에게 보여주기 => formState

```react
function ToDoList() {
  const {
    register,
    handleSubmit,
    formState: { errors }, // 에러 표시하기
  } = useForm<IForm>({
    defaultValues: {
      email: "@naver.com", // 기본 값으로 @naver.com 설정
    },
  });
  const onValid = (data: any) => {
    //onSubmit={handleSubmit(onValid)} 를 위해서 반드시 필요한 인자
    console.log(data);
  };
  return (
    <div>
      <form
        style={{ display: "flex", flexDirection: "column" }}
        onSubmit={handleSubmit(onValid)}
      >
        <input
          {...register("email", {
            required: "Email is required", // 입력이 되지 않으면 나올 메세지
            pattern: {
              value: /^[A-Za-z0-9._%+-]+@naver.com$/,  // 정규표현식으로 @naver.com
              message: "Only naver.com emails allowed", // 틀릴 시 메세지
            },
          })}
          placeholder="Email"
        />
        <span>{errors?.email?.message}</span> // errors.email 메세지가 있다면 출력
        <input
          {...register("firstName", { required: "write here" })}
          placeholder="First Name"
        />
        <span>{errors?.firstName?.message}</span>
        <input
          {...register("lastName", { required: "write here" })}
          placeholder="Last Name"
        />
        <span>{errors?.lastName?.message}</span>
        <input
          {...register("username", { required: "write here", minLength: 10 })}
          placeholder="Username"
        />
        <span>{errors?.username?.message}</span>
        <input
          {...register("password", { required: "write here", minLength: 5 })}
          placeholder="Password"
        />
        <span>{errors?.password?.message}</span>
        <input
          {...register("password1", {
            required: "Password is required",
            minLength: {
              value: 5,
              message: "Your password is too short.",
            },
          })}
          placeholder="Password1"
        />
        <span>{errors?.password1?.message}</span>
        <button>Add</button>
      </form>
    </div>
  );
}
```

Validation에는 key: value 로 지정하는 방법이 있고 

key : Object로 지정하는 방법이 있다.

```react
<input
    {...register("email", {
        required: "Email is required", // 입력이 되지 않으면 나올 메세지
        pattern: {
            value: /^[A-Za-z0-9._%+-]+@naver.com$/,  // 정규표현식으로 @naver.com
            message: "Only naver.com emails allowed", // 틀릴 시 메세지
        },
    })}
    placeholder="Email"
    />
```

위의 방법들을 이용해서 쉽게 form에서 사용자로부터 받을 정보들을 validate 할 수 있다.

=> 추가적으로 조건을 충족하지 못했을 경우 error?.value?.message까지 출력 가능



특정 항목의 조건에 따라 에러를 발생시키는 방법(입력받은 두 개의 비밀번호 일치여부 판단)

=> setError

form의 최종형태!!!!

```react
// 회원가입 form 최종 형태 

import { useForm } from "react-hook-form";

interface IForm {
  email: string;
  firstName: string;
  lastName: string;
  username: string;
  password: string;
  password1: string;
  extraError?: string;
}

function ToDoList() {
  const {
    register,
    handleSubmit,
    formState: { errors },
    setError,					// setError를 통해서 직접 에러를 발생시킴
  } = useForm<IForm>({
    defaultValues: {
      email: "@naver.com",
    },
  });
  const onValid = (data: IForm) => {
    if (data.password !== data.password1) {	// 입력받은 비밀번호가 일치하지 않는다면
      setError(	// 에러 발생 트리거
        "password1",
        { message: "Password are not the same" }, // 에러 메세지
        { shouldFocus: true }					// 에러가 발생한 input으로 자동 focus
      );
    }
    // setError("extraError", { message: "Server offline." });
  };
  console.log(errors);
  return (
    <div>
      <form
        style={{ display: "flex", flexDirection: "column" }}
        onSubmit={handleSubmit(onValid)}
      >
        <input
          {...register("email", {
            required: "Email is required",
            pattern: {
              value: /^[A-Za-z0-9._%+-]+@naver.com$/,
              message: "Only naver.com emails allowed",
            },
          })}
          placeholder="Email"
        />
        <span>{errors?.email?.message}</span>
        <input
          {...register("firstName", {
            required: "write here",
            validate: { // validate을 통해 원하지 않는 입력 값 배제
              noNico: (value) => // value는 현재 사용자가 입력한 값
                value.includes("nico") ? "no nicos allowed" : true, // 포함하면 no nic.. 메세지를 에러메세지로 보여줌
              noNick: (value) =>
                value.includes("nick") ? "no nick allowed" : true,
            },
          })}
          placeholder="First Name"
        />
        <span>{errors?.firstName?.message}</span>
        <input
          {...register("lastName", { required: "write here" })}
          placeholder="Last Name"
        />
        <span>{errors?.lastName?.message}</span>
        <input
          {...register("username", { required: "write here", minLength: 10 })}
          placeholder="Username"
        />
        <span>{errors?.username?.message}</span>
        <input
          {...register("password", { required: "write here", minLength: 5 })}
          placeholder="Password"
        />
        <span>{errors?.password?.message}</span>
        <input
          {...register("password1", {
            required: "Password is required",
            minLength: {
              value: 5,
              message: "Your password is too short.",
            },
          })}
          placeholder="Password1"
        />
        <span>{errors?.password1?.message}</span>
        <button>Add</button>
        <span>{errors?.extraError?.message}</span>
      </form>
    </div>
  );
}
export default ToDoList;
```



입력받은 두 비밀번호의 일치 여부 판단

```react
const onValid = (data: IForm) => {
    if (data.password !== data.password1) {	// 입력받은 비밀번호가 일치하지 않는다면
      setError(	// 에러 발생 트리거
        "password1",
        { message: "Password are not the same" }, // 에러 메세지
        { shouldFocus: true }					// 에러가 발생한 input으로 자동 focus
      );
    }
  };
```



원하지 않는 값은 입력을 받지 않고 해당 오류 메세지(제약조건)을 보여준다

```react
<input {...register("firstName", {
   required: "write here",
   validate: { // validate을 통해 원하지 않는 입력 값 배제
     noNico: (value) => // value는 현재 사용자가 입력한 값
       value.includes("nico") ? "no nicos allowed" : true, 
       // 포함하면 no nic.. 메세지를 에러메세지로 보여줌
     noNick: (value) =>
        value.includes("nick") ? "no nick allowed" : true,
        },
     })}
    placeholder="First Name"/>
```



==============================================================================

