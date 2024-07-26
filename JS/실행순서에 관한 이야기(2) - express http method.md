  앞서 다루던 자바스크립트 실행순서에 이어서 이번엔 express의 http method의 실행 순서에 대해서 다뤄보겠다.

```js
const express = require("express");

const app = express();

app.post("/users/login", async (req, res) => {
  try {
    const { email, password } = req.body;

    const [user] = await AppDataSource.query(
      `
        SELECT *
        FROM users u
        WHERE u.email = ?
      `,
      [email]
    );
  } catch (err) {
    res.status(err.statusCode || 401).json({ message: err.message });
  }
});

const startServer = async () => {
  const PORT = process.env.PORT;

  app.listen(PORT, () => {
    console.log(`Listening on Port ${PORT}`);
  });

  await AppDataSource.initialize();
  console.log(await AppDataSource.query(`select * from users;`)); // ---(1)
  // 잘 표시된다
};

startServer();

console.log(await AppDataSource.query(`select * from users;`)); // ---(2)
// TypeORMError: Connection is not established with mysql database
```

 (1)은 mysql 데이터베이스에서 users라는 테이블을 잘 불러와서 표시해준다. 하지만 (2)의 경우 데이터베이스와 연결이 되지 않았다고 에러메시지가 표시된다. 분명 서버를 시작하고 데이터베이스와 연결을 해주는 startServer함수는 (1)보다 아래쪽에 있고 (2)보다 윗쪽에 있는데도 말이다.  
 
 그 이유는 자바스크립트는 동기식으로 실행되는 것이 맞지만 node.js는 비동기 기반의 프로그래밍을 강조하는 플랫폼으로 실행을 완료하는데 오래걸리면 바로 다음 작업을 실행한다. startServer함수가 비동기적으로 실행되었다는 뜻이다. 그래서 이를 위해 데이터베이스 연결까지 다 완료된 후에 테이블을 표시하도록 (1)은startServer함수 안에 await를 이용해서 순서를 지정해 준것이다. 그에 반해 (2)는 startServer와 순서를 정해주지 않았기 때문에 코드는 아래쪽에 있지만 먼저 실행된것이다.  
 그렇다면 한가지 더 의문이 생긴다. app.post 메서드는 await나 then과 같은 비동기적 실행순서를 정해주는 무엇도 없는데 왜 에러가 발생하지 않을까??
 그 이유는 express의 htttp 메서드는 지정된 uri로 정해진 request가 왔을때만 실행되도록 설계되었기 때문이다.