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