## Axios 요청 방법

```js
const baseUrl = "http://localhost:8080"; // 기본 url  

function signin(event) {
    event.preventDefault(); // submit 시에 새로고침 방지하는 것 => 새로고침되면 입력값이 사라짐 ㅂㄷ
	
    // axios 요청하려면 npm install axios 필수!!
    const signin = async () => { // axios 요청 
      await axios({
        url: "/users/sign-in", // 기본 url에 추가로 붙음 => 							 						http://localhost:8080/users/sign-in
        method: "post", // 요청 method => get, post, put, delete 선택
        data: {			// 보낼 데이터 key: value 형태로
          email: email,
          password: password,
        },
        baseURL: baseUrl,	// 위에서 말한 baseURL 설정
      })	// 여기까지가 axios 보내는 정보
        .then((response) => { // .then은 axios 요청 성공시 작업
          console.log(response.data); // 성공시 뭐가 response에 찍혔나 확인해봤음...
          setEmail(""); // input창에 적혀있던 아이디는 "" 값으로 바꿔놓기
          setPassword(""); // 위와 같음
        })
        .catch((error) => { // .catch는 axios 요청 실패시 작업
          console.log(error); // error 메세지 확인
        });
    };
    signin(); // 방금 만든 axios fucntion 실행하기
  }

// 그러면 해당 signin 함수는 <form>의 onSubmit에서 요청된다.
<LoginForm onSubmit={signin}> // 이렇게 요청함
```

