

기존의 selector을 이용한 state와 연결 후 상태관리

```typescript
// App.tsx
import React from "react";
import { useRecoilState, useRecoilValue } from "recoil";
import { hourSelector, minuteState } from "./atoms";

function App() {
  const [minutes, setMinutes] = useRecoilState(minuteState);
  const hours = useRecoilValue(hourSelector);
  const onMinutesChange = (event: React.FormEvent<HTMLInputElement>) => {
    setMinutes(+event.currentTarget.value);
    // event.currentTarget.value 값은 string이기 때문에 앞에 +를 붙여주면 number로 바뀐다.
  };
  return (
    <div>
      <input
        value={minutes}
        onChange={onMinutesChange}
        type="number"
        placeholder="Minutes"
      />
      <input value={hours} type="number" placeholder="Hours" />
    </div>
  );
}

export default App;

// atoms.tsx
import { atom, selector } from "recoil";

// atom으로 선언한 state 저장소
export const minuteState = atom({
  key: "minutes",
  default: 0,
});

// selector를 통해서 minuteState와 연결후 분으로 계산한 결과를 return
export const hourSelector = selector({
  key: "hours",
  get: ({ get }) => { // get 함수를 통해서 연결
    const minutes = get(minuteState);
    return minutes / 60;
  },
});
```



selector의 두 번째 property인 set함수에 대해...!

이전에 get 함수를 통해서 atom의 값과 연결했다

selector의 두 번째 property인 set을 이용하면 해당 selector을 통해서 atom값을 수정할 수 있다.

=> 기존에 get을 이용하면 atom값은 그대로있지만 output만 바꿨었음

```typescript
// App.tsx
import React from "react";
import { useRecoilState, useRecoilValue } from "recoil";
import { hourSelector, minuteState } from "./atoms";

function App() {
  const [minutes, setMinutes] = useRecoilState(minuteState);
  // const hours = useRecoilValue(hourSelector);
  const [hours, setHours] = useRecoilState(hourSelector);
  const onMinutesChange = (event: React.FormEvent<HTMLInputElement>) => {
    setMinutes(+event.currentTarget.value);
    // event.currentTarget.value 값은 string이기 때문에 앞에 +를 붙여주면 number로 바뀐다.
  };
  const onHoursChange = (event: React.FormEvent<HTMLInputElement>) => {
    setHours(+event.currentTarget.value);
  };
  return (
    <div>
      <input
        value={minutes}
        onChange={onMinutesChange}
        type="number"
        placeholder="Minutes"
      />
      <input
        onChange={onHoursChange}
        value={hours}
        type="number"
        placeholder="Hours"
      />
    </div>
  );
}

export default App;

// atoms.tsx
import { atom, selector } from "recoil";

// atom으로 선언한 state 저장소
export const minuteState = atom({
  key: "minutes",
  default: 0,
});

// selector를 통해서 minuteState와 연결후 분으로 계산한 결과를 return
export const hourSelector = selector<number>({
  key: "hours",
  get: ({ get }) => {
    // get 함수를 통해서 연결
    const minutes = get(minuteState);
    return minutes / 60;
  },
  set: ({ set }, newValue) => {
    const minutes = Number(newValue) * 60; // setHours를 통해서 받은 hours에 60을 곱한 값 => minutes
    set(minuteState, minutes); // minutes를 기존의 minuteState의 값으로 바꾼다.
  },
});
```



===============================================================================

드래그와 드롭이 가능한 component만들기

react-beautiful-dnd라는 라이브러리 다운로드

```shell
npm i react-beautiful-dnd
npm i --save-dev @types/react-beautiful-dnd
```



DragDropContext => Droppable => Draggable 순으로 작성

```typescript
// App.tsx
import { DragDropContext, Droppable, Draggable } from "react-beautiful-dnd";

function App() {
  const onDragEnd = () => {};
  return (
    <DragDropContext onDragEnd={onDragEnd}>
      <div>
        <Droppable droppableId="one">
          {() => ( // children은 함수형으로 작성한다.
            <ul>
              <Draggable draggableId="first" index={0}>
                {() => <li>One</li>}
              </Draggable>
              <Draggable draggableId="second" index={1}>
                {() => <li>Two</li>}
              </Draggable>
            </ul>
          )}
        </Droppable>
      </div>
    </DragDropContext>
  );
}

export default App;
```



=> {() => `<li>`One`</li>`} 처럼 children을 함수형으로 작성해야하는데 이때 {(magic)=>()}를 적어주고 magic는 다양한 인자들을 가지고 있다.

```typescript
// App.tsx
import { DragDropContext, Droppable, Draggable } from "react-beautiful-dnd";

function App() {
  const onDragEnd = () => {};
  return (
    <DragDropContext onDragEnd={onDragEnd}>
      <div>
        <Droppable droppableId="one">
          {(magic) => (
            <ul ref={magic.innerRef} {...magic.droppableProps}> //{...magic.droppableProps} : 움직일 수 있게 해줌
              <Draggable draggableId="first" index={0}>
                {(magic) => (
                  <li ref={magic.innerRef} {...magic.draggableProps}> // {...magic.draggableProps} : 움직일 수 있게 해줌
                    <span {...magic.dragHandleProps}>🔥</span>  /// {...magic.dragHandleProps} <span>손잡이</span> => span안에 있는 요소를 눌러서 움직일 수 있음 == handle
                    One
                  </li>
                )}
              </Draggable>
            </ul>
          )}
        </Droppable>
      </div>
    </DragDropContext>
  );
}

export default App;
```



DragDropContext => Boards => Droppable => Board => Draggable => Card 순으로 작성

=> 위의 형태를 좀 더 trello와 비슷하게 변형 => 하지만 여전히 위치를 이동시킨 div가 제자리로 돌아온다

=> onDragEnd() 함수를 설정하지 않았기 때문

```typescript
// App.tsx
import { DragDropContext, Droppable, Draggable } from "react-beautiful-dnd";
import styled from "styled-components";

const Wrapper = styled.div`
  display: flex;
  max-width: 480px;
  width: 100%;
  margin: 0 auto;
  justify-content: center;
  align-items: center;
  height: 100vh;
`;

const Boards = styled.div`
  display: grid;
  width: 100%;
  grid-template-columns: repeat(1, 1fr);
`;

const Board = styled.div`
  padding: 20px 10px;
  padding-top: 30px;
  background-color: ${(props) => props.theme.boardColor};
  border-radius: 5px;
  min-height: 200px;
`;

const Card = styled.div`
  border-radius: 5px;
  margin-bottom: 5px;
  padding: 10px 10px;
  background-color: ${(props) => props.theme.cardColor};
`;

const toDos = ["a", "b", "c", "d", "e", "f"];

function App() {
  const onDragEnd = () => {};
  return (
    <DragDropContext onDragEnd={onDragEnd}>
      <Wrapper>
        <Boards>
          <Droppable droppableId="one">
            {(magic) => (
              <Board ref={magic.innerRef} {...magic.droppableProps}>
                {toDos.map((toDo, index) => (
                  <Draggable draggableId={toDo} index={index}>
                    {(magic) => (
                      <Card
                        ref={magic.innerRef}
                        {...magic.dragHandleProps}
                        {...magic.draggableProps}
                      >
                        {toDo}
                      </Card>
                    )}
                  </Draggable>
                ))}
                {magic.placeholder} // {magic.placeholder}가 없으면 내부 div component(Card)를 drag 했을 때 Board의 크기가 일시적으로 줄어든다
              </Board>
            )}
          </Droppable>
        </Boards>
      </Wrapper>
    </DragDropContext>
  );
}

export default App;
```

 참고 : CSS display grid는 => https://studiomeal.com/archives/533 참고bb

JS의 splice() 함수에 대해

=> splice()는[ ] 안에서 원하는 element를 삭제하고 

=> [ ]안에 원하는 element를 넣을 수 있도록 도와주는 함수다.

x = ["a", "b", "c", "d", "e"]

x.splice(1, 1); => x에서 index = 1 값으로 시작해서 1개를 삭제한다

x.splice(3, 0, "b"); => x에서 index = 3 인 위치에 "b"를 넣는다

=> x = ["a", "c", "d", "b", "e"]



배열이든 문자열이든 함수를 적용했을때 원본이 바뀌면 => mutation이라고 함

```typescript
// App.tsx
function App() {
  const [toDos, setToDos] = useRecoilState(toDoState);
  const onDragEnd = ({ draggableId, destination, source }: DropResult) => { // 중요
    if (!destination) return;
    setToDos((oldToDos) => {
      const toDosCopy = [...oldToDos];	// mutation하지 않기 위해 복사
      // 1) Delete item on source.index
      // console.log("Delete item on", source.index);
      // console.log(toDosCopy);
      toDosCopy.splice(source.index, 1); // 기존의 위치에 있는 element 삭제
      // console.log("Deleted item");
      // console.log(toDosCopy);
      // 2) Put back the item on the destination.index
      // console.log("Put back", draggableId, "on ", destination.index);
      toDosCopy.splice(destination?.index, 0, draggableId); // 도착지점에 draggableId 넣기
      // console.log(toDosCopy);
      return toDosCopy;
    });
  };
  return (
    <DragDropContext onDragEnd={onDragEnd}>
      <Wrapper>
        <Boards>
          <Droppable droppableId="one">
            {(magic) => (
              <Board ref={magic.innerRef} {...magic.droppableProps}>
                {toDos.map((toDo, index) => (
                  <Draggable key={toDo} draggableId={toDo} index={index}> // 일반적으로 map의 key는 index(number)를 사용하지만 beautiful-dud에서는 draggableId와 맞추는 것이 규칙이다. => key={toDo} draggableId={toDo}
                    {(magic) => (
                      <Card
```



=> react는 기본적으로 state가 바뀌면 리랜더링한다 => 이때 parent의 state가 바뀌면 children들 전체가 전부 다시 리랜더링된다 => 낭비 => 위의 페이지에서 Card 하나를 움직이면 엄청나게 많은 리랜더링이 되는 이유!!

=> react memo에게 해당 component는 리랜더링 하지 말라고 도움을 요청하면 해결됨.



React.memo의 역할 => props가 바뀌지 않는 component는 리랜더링하지 않는다!!!

=> 최적화 기술!!

```typescript
// DragabbleCard.tsx
import React from "react";
import { Draggable } from "react-beautiful-dnd";
import styled from "styled-components";

const Card = styled.div`
  border-radius: 5px;
  margin-bottom: 5px;
  padding: 10px 10px;
  background-color: ${(props) => props.theme.cardColor};
`;

interface IDragabbleCardProps {
  toDo: string;
  index: number;
}

function DragabbleCard({ toDo, index }: IDragabbleCardProps) {
  console.log(toDo, "has been rendered");
  return (
    <Draggable key={toDo} draggableId={toDo} index={index}>
      {(magic) => (
        <Card
          ref={magic.innerRef}
          {...magic.dragHandleProps}
          {...magic.draggableProps}
        >
          {toDo}
        </Card>
      )}
    </Draggable>
  );
}

export default React.memo(DragabbleCard); // React.memo를 통해서 불필요한 랜더링 제거
```



=> Multi Boards로 만들기 => [Todo, Doing, Done]

```typescript
// App.tsx
function App() {
  const [toDos, setToDos] = useRecoilState(toDoState);
  const onDragEnd = ({ draggableId, destination, source }: DropResult) => {
    if (!destination) return;
    /* setToDos((oldToDos) => {
        const toDosCopy = [...oldToDos];
        toDosCopy.splice(source.index, 1);
        toDosCopy.splice(destination?.index, 0, draggableId);
        return toDosCopy;
      });
      }); */
  };

  return (
    <DragDropContext onDragEnd={onDragEnd}>
      <Wrapper>
        <Boards>
          {Object.keys(toDos).map((boardId) => (
            <Board boardId={boardId} key={boardId} toDos={toDos[boardId]} />
          ))}
        </Boards>
      </Wrapper>
    </DragDropContext>
  );
}

export default App;
```

Boards는 안에 내부적으로 key : [ ... ] 형태로 되어 있다.

따라서 내부에 존재하는 각각의 array를 출력하기 위해서는

```typescript
<Boards>
    {Object.keys(toDos).map((boardId) => (
        <Board boardId={boardId} key={boardId} toDos={toDos[boardId]} />
	))}
</Boards>
```

이제 각각의 Board안에서 component가 이동하는 기능이 사라졌다...



=> 다시 이동하면 해당 위치에 고정되는 기능 만들기

```typescript
// App.tsx
function App() {
  const [toDos, setToDos] = useRecoilState(toDoState);
  const onDragEnd = (info: DropResult) => {
    console.log(info);
    const { destination, draggableId, source } = info;
    if (destination?.droppableId === source.droppableId) { // 출발 도착 확인
      // same board movement.
      setToDos((allBoards) => {
        const boardCopy = [...allBoards[source.droppableId]]; // 수정된 Board복사
        boardCopy.splice(source.index, 1); // 기존의 이동과정 함수
        boardCopy.splice(destination?.index, 0, draggableId); // 기존의 이동과정 함수
        return { // return 할 때, 기존의 Board(바뀌지 않은) + 바뀐 Board(이동완료)
          ...allBoards, // 기존의 Boards
          [source.droppableId]: boardCopy, // 이동이 완료된 Board
        };
      });
    }
  };

  return (
```

1. onDragEnd : drag and drop가 끝난 뒤에 어떤 일이 일어나는지 onDragEnd를 통해 가져온다
2. destination?.droppableId === source.droppableId :  출발지와 도착지점이 같은지 확인한다
3. 이동한 component가 있는 Board를 복사한다
4. 기존의 component가 이동했을 때 logic를 적용한다
5. 최종적으로 return 할 때, Object형태 였기 때문에 기존의 Board(이동X) + 이동이 완료된 Board를 return한다.