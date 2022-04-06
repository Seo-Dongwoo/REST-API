# REST-API

### postman을 이용해서 data를 생성, 수정, 삭제

### frontend의 요청을 backend에서 처리한 후 다시 frontend로 전송해주는 알고리즘

```
const express = require("express");
const app = express();

const database = [
  { id: 1, title: "1번입니다." }, // database[0]
  { id: 2, title: "2번입니다." },
  { id: 3, title: "3번입니다." },
];

// body에 담겨져오는 것을 읽으려면 bodyparser을 express에 추가해줘야한다.
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// http://localhost:3000
// 데이터를 가져올 때는 get을 쓴다.
app.get("/", function (req, res) {
  res.sendFile(__dirname + "/views/index.html");
});

// postman은 API 개발을 보다 빠르고 쉽게 구현 할 수 있도록 도와주며,
// 개발된 API를 테스트하여 문서화 또는 공유 할 수 있도록 도와 주는 플랫폼이다.
app.get("/database", function (req, res) {
  res.send(database);
});

app.get("/database/:id", function (req, res) {
  const id = req.params.id; // 여기서 id는 type이 string
  const data = database.find((el) => el.id === Number(id)); // 여기서는 number
  res.send(data);
});

// REST API
// 생성 : post
// 수정 : put, patch
// 삭제 : delete

// 데이터 추가
app.post("/database", function (req, res) {
  const title = req.body.title;
  database.push({
    id: database.length + 1,
    title,
  });
  res.send("데이터 값 추가가 정상적으로 완료되었습니다.");
});

// 데이터 수정
app.put("/database", function (req, res) {
  const id = req.body.id;
  const title = req.body.title;
  database[id - 1].title = title;
  // id - 1 은 database[]의 index
  res.send("데이터 값 수정이 완료되었습니다.");
});

// 데이터 삭제
app.delete("/database", function (req, res) {
  const id = req.body.id;
  database.splice(id - 1, 1);
  res.send("데이터 값 삭제가 완료되었습니다.");
});

app.listen(3000, () => {
  console.log("server on!");
});
```
