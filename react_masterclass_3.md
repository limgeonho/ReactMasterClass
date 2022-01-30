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

