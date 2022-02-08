## Axios로 file을 보내는 방법(jquery X)

```javascript
  // put으로 사용자가 입력한 내용 보내기 (json + file) 한 번에 보내는 방법
  // $ : jquery를 사용하기 위해서는 npm install jquery 해야함 => useRef으로 대체 가능
  const formRef = useRef(null); // useRef Hook를 통해서 form 태그 잡아오기
  
  const updateCreator = (event) => {
    event.preventDefault(); // 새로고침 막기
    const data = {
      // json 데이터 만들기
      creatorName: creatorName,
      creatorImageUrl: creatorImageUrl,
      creatorContent: creatorContent,
      email: email,
      tel: tel,
      account: account,
    };

    // const form = $("#form")[0]; =>  input file을 가지고 있는 form
    const form = formRef[0]; // useRef을 통해서 jquery대신 form태그를 잡아오는 방법
    const formData = new FormData(form); // new formData() 만들기 : file을 보내려면 필수!

    formData.append(
      "creator",
      new Blob([JSON.stringify(data)], { type: "application/json" })
      // 위에서 json으로 만든 data를 넣고 new Blob(미디어파일을 보내기 위함)에 넣기 type도 지정
    );
    formData.append("creatorImage", $("#file")[0].files[0]);
    //formData.append("creatorImage", null);

    const updateCreator = async () => {
      await axios({
        url: `/creators/${projectId}`,
        method: "put",
        data: formData, // 만들어 놓은 formData를 data값으로 넣기 끝!!
        baseURL: baseUrl,
        processData: false,
        contentType: false,
      })
        .then((response) => {
          console.log("성공");
          console.log(response.data);
        })
        .catch((error) => {
          console.log("에러발생");
          console.log(error);
        });
    };
    updateCreator();
  };

```

- 추가적으로 html코드에는 

  - jquery를 사용 O
    - `<form>`에 *id*="form" *enctype*="multipart/form-data" 설정
  - jquery를 사용 X
    - `<form>`에 ref={formRef} *enctype*="multipart/form-data" 로 설정
  - `<input>`에는 type="file" id="file" name="file" 설정

- 참고사이트

  https://medium.com/jaehoon-techblog/simpleblog-%EA%B0%9C%EB%B0%9C-%EC%9D%BC%EC%A7%80-4-55a8d2a8604