## Framer Animation



frammer 설치 방법

```shell
npm i framer-motion
```

framer 설치시 version이 4로 설정되어 있을 경우에 error발생 => version5로 override해야함

```shell
# 버전 오류시 CRACO 설치
npm install @craco/craco --save
# package.json 파일과 같은 위치에 craco.config.js파일 만들기
```

```js
// craco.config.js
module.exports = {
  webpack: {
    configure: {
      module: {
        rules: [
          {
            type: "javascript/auto",
            test: /\.mjs$/,
            include: /node_modules/,
          },
        ],
      },
    },
  },
};
```

```json
// package.json
  "scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```



framer를 사용하기 위해서는 일반 HTML 태그안에서 사용할 수 없다

```html
`<div>``</div>` => 사용불가
`<motion.div>``</motion.div>` => 으로 사용
```



기존의 styled-component와 함께 사용하고 싶다면?

```typescript
const Box = styled(motion.div)` // styled(motion.div) 형태로 작성
  width: 200px;
  height: 200px;
  background-color: white;
  border-radius: 15px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;
```

```typescript
// App.tsx
function App() {
  return (
    <Wrapper>
      <Box
        transition={{ type: "spring", delay: 0.5 }} // 기본 변화 설정 spring
        initial={{ scale: 0 }}					  // 초기모습 initial scale = 0 안보임
        animate={{ scale: 1, rotateZ: 360 }}	   // 최종모습 animate scale = 1 보임
      />
      <motion.div></motion.div>
    </Wrapper>
  );
}

export default App;
```



Variants(1)

=> 원하는 애니메이션 설정들을 모아 놓을 수 있음

```typescript
// App.tsx
const myVars = {
  // 원하는 설정들을 모아 놓음
  start: { scale: 0 },
  end: { scale: 1, rotateZ: 360, transition: { type: "spring", delay: 0.5 } },
};

function App() {
  return (
    <Wrapper>
      <Box variants={myVars} initial="start" animate="end" /> // variants 설정
      <motion.div></motion.div>
    </Wrapper>
  );
}

export default App;
```



Variants(2)

=> 부모의 Variants를 자식이 상속받을 수 있다.

=> Motion만의 함수, 특징이 존재한다(ex, delayChildren, staggerChildren)

대부분은 css코드와 같지만 몇몇 Motion만의 코드가 존재한다

```typescript
// App.tsx
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Circle = styled(motion.div)`
  background-color: white;
  height: 70px;
  width: 70px;
  place-self: center;
  border-radius: 35px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  start: {
    opacity: 0,
    scale: 0.5,
  },
  end: {
    scale: 1,
    opacity: 1,
    transition: {
      type: "spring",
      duration: 0.5,
      bounce: 0.5,
      delayChildren: 0.5, // 자식 component가 나타나는 시점
      staggerChildren: 0.2, // 자식들이 여려개일 경우 component사이의 등장 시간 간격 
    },
  },
};

const circleVariants = {
  start: {
    opacity: 0,
    y: 10,
  },
  end: {
    opacity: 1,
    y: 0,
  },
};

function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} initial="start" animate="end">
        <Circle variants={circleVariants} /> // 부모의 boxVariants 상속 + circleVariants
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}

export default App;
```



Gestures(1)

=> hover, focus 등... 특정 조건에 따라서 움직이고 싶은 경우

whileHover, whileDrag, whileTap(=click)

```typescript
// App.tsx
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  hover: { scale: 1.5, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
  drag: { backgroundColor: "rgb(46, 204, 113)", transition: { duration: 10 } },
};

function App() {
  return (
    <Wrapper>
      <Box
        drag // drag 할 수 있도록 만들어 주는 옵션
        variants={boxVariants} // variants의 initial과 animate의 사용법과 동일함
        whileHover="hover"
        whileDrag="drag"
        whileTap="click"
      />
    </Wrapper>
  );
}

export default App;
```



Gestures(2)

=> Gestures(1) 에서 드래그가 가능하게 한 component는 제약이 없는 상태이기 때문에 화면 밖으로 날아가 버린다...

=> drag에 제약조건을 추가하자

dragConstraints => 직접 영역 작성
허용된 드래그 가능 영역에 제약 조건을 적용합니다.

dragSnapToOrigin => 다시 원점으로 돌아오게하는 옵션
true인 경우 드래그 가능한 요소는 놓을 때 중심/원점으로 다시 애니메이션됩니다.

dragElastic => default는 0.5이고 1이면 가장 자유롭고 0이면 특정구역 밖으로 못나감
제한된 바깥을 벗어날 수 있는 이동 정도입니다.

```typescript
// App.tsx
const BiggerBox = styled.div`
  width: 600px;
  height: 600px;
  background-color: rgba(255, 255, 255, 0.4);
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const boxVariants = {
  hover: { rotateZ: 90 },
  click: { borderRadius: "100px" },
};

function App() {
  const biggerBoxRef = useRef<HTMLDivElement>(null); // biggerBoxRef로 HTML 태그 잡기

  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag
          dragSnapToOrigin // drag 후 다시 원점으로
          dragElastic={0.5} // drag 영역 밖에서 다시 원점으로 끌어당기는 힘 조절
          dragConstraints={biggerBoxRef} // useRef를 통해 잡은 HTML을 제한구역으로 설정
          variants={boxVariants}
          whileHover="hover"
          whileTap="click"
        />
      </BiggerBox>
    </Wrapper>
  );
}

export default App;
```



MotionValues(1)

=> 사용자가 component 에 변화를 주면 해당 변화들을 tracking한다

=> 하지만 가장 motionValue의 가장 중요한 점은 변화들을 tracking 하지만 리랜더링은 되지 않는다

=> 변화하더라도 리랜더링되지 않도록 설계되어 있음 => ? : 작은 변화에도 리랜더링하면 낭비 



MotionValues(2)

=> 사용자가 입력하는 특정 component의 값에 연결 => useMotionValue(0);

=> tracking 한 값을 이용해서 변화 => useTransform(x, [-800, 0, 800], [2, 1, 0.1]);

```typescript
// App.tsx
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const x = useMotionValue(0); // default = 0
  const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]); // useTransform(tracking 하고 있는 component값, input, output)
  return (
    <Wrapper>
      <Box style={{ x, scale }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
export default App;
```



MotionValues(3)

사용자가 scroll 했을 경우 변화를 감지해서 적용 => useViewportScroll();

```typescript
// App.tsx
const { scrollYProgress } = useViewportScroll();
const scale = useTransform(scrollYProgress, [0, 1], [1, 5]);  
// scrollYProgress 는 0 ~ 1 사이의 숫자로 이루어짐 => 화면의 Y축으로 몇 % 이동했는지?
```



```typescript
// App.tsx
const Box = styled(motion.div)`
  width: 200px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const x = useMotionValue(0);
  const rotateZ = useTransform(x, [-800, 800], [-360, 360]);
  const gradient = useTransform(
    x,
    [-800, 800],
    [
      "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
      "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
    ]
  );
  const { scrollYProgress } = useViewportScroll();
  const scale = useTransform(scrollYProgress, [0, 1], [1, 5]);
  return (
    <Wrapper style={{ background: gradient }}>
      <Box style={{ x, rotateZ, scale }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
export default App;
```



SVG animation

SVG? => scalable vector graphics

```typescript
// App.tsx
const Svg = styled.svg`
  width: 300px;
  height: 300px;
  path {
    stroke: white; 		// 마크의 테두리 색
    stroke-width: 2;	// 마크의 테두리 두께
  }
`;

const svg = {
  start: { pathLength: 0, fill: "rgba(255, 255, 255, 0)" }, // fill : 테두리 내부 채우기
  end: {
    fill: "rgba(255, 255, 255, 1)",
    pathLength: 1,
  },
};

function App() {
  return (
    <Wrapper>
      <Svg
        focusable="false"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 448 512"
      >
        <motion.path // path에 motion을 사용할 수 있도록 => motion.path 설정
          variants={svg} // variants 연결
          initial="start"
          animate="end"
          transition={{ // component 마다 다른 transition 시간을 부여하고 싶을 때 선언
            default: { duration: 5 }, // 기본 시간은 5초 설정
            fill: { duration: 1, delay: 3 }, // fill 요소만 시간을 다르게 설정하는 방법
          }}
          d="M224 373.12c-25.24-31.67-40.08-59.43-45-83.18-22.55-88 112.61-88 90.06 0-5.45 24.25-20.29 52-45 83.18zm138.15 73.23c-42.06 18.31-83.67-10.88-119.3-50.47 103.9-130.07 46.11-200-18.85-200-54.92 0-85.16 46.51-73.28 100.5 6.93 29.19 25.23 62.39 54.43 99.5-32.53 36.05-60.55 52.69-85.15 54.92-50 7.43-89.11-41.06-71.3-91.09 15.1-39.16 111.72-231.18 115.87-241.56 15.75-30.07 25.56-57.4 59.38-57.4 32.34 0 43.4 25.94 60.37 59.87 36 70.62 89.35 177.48 114.84 239.09 13.17 33.07-1.37 71.29-37.01 86.64zm47-136.12C280.27 35.93 273.13 32 224 32c-45.52 0-64.87 31.67-84.66 72.79C33.18 317.1 22.89 347.19 22 349.81-3.22 419.14 48.74 480 111.63 480c21.71 0 60.61-6.06 112.37-62.4 58.68 63.78 101.26 62.4 112.37 62.4 62.89.05 114.85-60.86 89.61-130.19.02-3.89-16.82-38.9-16.82-39.58z"
        ></motion.path>
      </Svg>
    </Wrapper>
  );
}
export default App;
```



Animate Presence(1)

Animate Presence를 사용하기 위해서는 component를 반드시 `<Animate Presence>`으로 감싸야 한다.

=> component가 나타나고 사라질때에 관한 animation을 설정할 수 있는 motion의 기능

기존의 component의 property에서 variants, initial, animate, + exit가 추가됨

=> exit는 해당 component가 사라질 때 나타나는 animaion을 설정

=> exit는 해당 component가 false일 때 알아서 실행된다

```typescript
// App.tsx
const boxVariants = {
  initial: {
    opacity: 0,
    scale: 0,
  },
  visible: {
    opacity: 1,
    scale: 1,
    rotateZ: 360,
  },
  leaving: {
    opacity: 0,
    scale: 0,
    y: 50,
  },
};

function App() {
  const [showing, setShowing] = useState(false);
  const toggleShowing = () => setShowing((prev) => !prev);
  return (
    <Wrapper>
      <button onClick={toggleShowing}>Click</button>
      <AnimatePresence> // 반드시 <AnimatePresence>로 묶어야 한다.
        {showing ? (
          <Box
            variants={boxVariants}
            initial="initial"	// 시작할때
            animate="visible"	// 나타날때
            exit="leaving"		// 사라질때 => 알아서 showing가 false일때 exit 옵션이 실행됨
          />
        ) : null}
      </AnimatePresence>
    </Wrapper>
  );
}
export default App;
```



Animate Presence(2) - slider[next]

component 슬라이더 만들기

과정은 `<AnimatePresence>`안에 원하는 component를 넣고 각각의 initial, animate, exit property 상태를 설정하는 부분에서 같다.

```typescript
// App.tsx
const Box = styled(motion.div)`
  width: 400px;
  height: 200px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  position: absolute;
  top: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 28px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const box = {
  invisible: {
    x: 500,
    opacity: 0,
    scale: 0,
  },
  visible: {
    x: 0,
    opacity: 1,
    scale: 1,
    transition: {
      duration: 1,
    },
  },
  exit: { x: -500, opacity: 0, scale: 0, transition: { duration: 1 } },
};

function App() {
  const [visible, setVisible] = useState(1);
  const nextPlease = () => setVisible((prev) => (prev === 10 ? 10 : prev + 1));
  const prevPlease = () => setVisible((prev) => (prev === 1 ? 1 : prev - 1));
  return (
    <Wrapper>
      <AnimatePresence> // <AnimatePresence>로 감싼다.
        {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map((i) => // Box component를 띄우는 map함수
          i === visible ? (
            <Box
              variants={box}
              initial="invisible"
              animate="visible"
              exit="exit"
              key={i}
            >
              {i}
            </Box>
          ) : null
        )}
      </AnimatePresence>
      <button onClick={nextPlease}>next</button> // 버튼을 통해서 조작
      <button onClick={prevPlease}>prev</button>
    </Wrapper>
  );
}
export default App;
```

=> 하지만 위의 component는 next와 prev를 눌렀을 때 Box가 바뀌기는 하지만 같은 방향에서 나타나고 사라진다 => 위의 문제를 해결해야함



Animate Presence(3) - slider[prev] => 양방향으로 움직일 수 있도록

component의 initial, animate, exit + custom이라는 property를 추가한다

custom="custom" => 원하는 설정을 따로 할 수 있음

custom ={

} 안에 back이라는 인자를 true/false로 연결해서 버튼이 눌릴시 마다 방향을 전환한다

back? true일 경우 : false일 경우 선택 후 적용

=> 소스코드는 생략



layout

=> component에 적용할 수 있는 framer motion의 강력한 property

=> layout를 적용하면 다른 컴포넌트와 자연스럽게 연결할 수 있고 => framer motion이 알아서 부드러운 animation을 적용해준다.

```typescript
// App.tsx

const Box = styled(motion.div)`
  width: 400px;
  height: 400px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Circle = styled(motion.div)`
  background-color: #00a5ff;
  height: 100px;
  width: 100px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const [clicked, setClicked] = useState(false);
  const toggleClicked = () => setClicked((prev) => !prev);
  return (
    <Wrapper onClick={toggleClicked}>
      <Box
        style={{
          justifyContent: clicked ? "center" : "flex-start",
          alignItems: clicked ? "center" : "flex-start",
        }}
      >
        <Circle layout /> // layout 속성을 이용해서 onclick의 상태에 따라 animation연결!
      </Box>
    </Wrapper>
  );
}
export default App;
```

=> 기본적으로 React.js는 모든 component를 다른 각각의 component로 인식한다

하지만 layoutId를 통해서 서로 다른 두개의 component를 하나의 component로 묶는 다면 알아서 framer motion이 하나로 연결된 component에 animation을 적용해 준다.

=> `<Circle layoutId="circle" style={{ borderRadius: 50 }} />`

```typescript
// App.tsx
const Box = styled(motion.div)`
  width: 400px;
  height: 400px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Circle = styled(motion.div)`
  background-color: #00a5ff;
  height: 100px;
  width: 100px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

function App() {
  const [clicked, setClicked] = useState(false);
  const toggleClicked = () => setClicked((prev) => !prev);
  return (
    <Wrapper onClick={toggleClicked}>
      <Box>
        {!clicked ? (
          <Circle layoutId="circle" style={{ borderRadius: 50 }} /> // component 묶기
        ) : null}
      </Box>
      <Box>
        {clicked ? (
          <Circle layoutId="circle" style={{ borderRadius: 0, scale: 2 }} />
        ) : null}
      </Box>
    </Wrapper>
  );
}
export default App;
```



Final Animation

=> 사용자가 원하는 상자를 클릭시에 주변 화면이 까매지면서 해당 상자만 확대되는 animation작성

1. `<AnimatePresence>`를 통해서 component의 initial, animate, exit 상태를 각각 정하고

2. layoutId 를 통해서 원하는 component들을 하나의 component상태로 연결 => framer motion이 알아서 animation연결을 해줌

```typescript
// App.tsx
import styled from "styled-components";
import { motion, AnimatePresence } from "framer-motion";
import { useState } from "react";

const Wrapper = styled.div`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: space-around;
  align-items: center;
`;

const Grid = styled.div`
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 50vw;
  gap: 10px;
  div:first-child,
  div:last-child {
    grid-column: span 2;
  }
`;

const Box = styled(motion.div)`
  background-color: rgba(255, 255, 255, 1);
  border-radius: 40px;
  height: 200px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1), 0 10px 20px rgba(0, 0, 0, 0.06);
`;

const Overlay = styled(motion.div)`
  width: 100%;
  height: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
`;

const overlay = {
  hidden: { backgroundColor: "rgba(0, 0, 0, 0)" },
  visible: { backgroundColor: "rgba(0, 0, 0, 0.5)" },
  exit: { backgroundColor: "rgba(0, 0, 0, 0)" },
};

function App() {
  const [id, setId] = useState<null | string>(null);

  return (
    <Wrapper>
      <Grid>
        {["1", "2", "3", "4"].map((n) => (
          <Box onClick={() => setId(n)} key={n} layoutId={n} /> // onClick={() => setId(n)} : 반드시 () => fuction 형태로 작성해야 클릭했을 경우 fuction을 실행한다, 그러지 않으면 바로 실행해버림;;
        ))}
      </Grid>
      <AnimatePresence> // initial, animate, exit를 위해서 <AnimatePresence>로 감싸기
        {id ? (
          <Overlay
            variants={overlay}
            onClick={() => setId(null)} // Box이외의 공간을 클릭시에 다시 null 설정
            initial="hidden"
            animate="visible"
            exit="exit"
          >
            <Box layoutId={id} style={{ width: 400, height: 200 }} /> // layoutId={id}을 통해서 클릭시 Box와 연결 => 알아서 animation이 적용됨
          </Overlay>
        ) : null}
      </AnimatePresence>
    </Wrapper>
  );
}
export default App;
```



