ts파일에서 gql 문법을 사용할 때 오타나 타입오류가 없는지 확인하기 위해서 `@grapgql-codegen/cli`라는 라이브러리를 먼저 설치해야한다.
`npm i -D grapqhql @graphql-codegen/cli`
여기서 grqphql은 전역설치가 되지않도록 주의하자. 여러 중복된 grqphql 버전으로 인해 오류를 낼 가능성이 있다고 한다.
그다음 프로젝트의 루트폴더에 `codegen.ts` 파일을 생성해주고 아래와 같이 작성해주자. 옵션에 관한 것은
[`이곳`](https://www.the-guild.dev/graphql/codegen/docs/config-reference/codegen-config)을 참조하자.
```ts
import { CodegenConfig } from '@graphql-codegen/cli';

const config: CodegenConfig = {
  schema: '<URL_OF_YOUR_GRAPHQL_API>',
  // this assumes that all your source files are in a top-level `src/` directory - you might need to adjust this to your file structure
  documents: ['src/**/*.{ts,tsx}'],
  generates: {
    './src/__generated__/': {
      preset: 'client',
      plugins: [],
      presetConfig: {
        gqlTagName: 'gql',
      }
    }
  },
  ignoreNoDocuments: true,
};

export default config;
```

이제 pacakege.json에 다음과 같은 스크립트를 추가하자
```json
"scripts": {
	"graphql:codegen": "graphql-codegen"
}
```

그다음으로 `npm run graphql:codegen` 명령어를 실행시켜주면 `__generated__`라는 디렉토리에 백엔드서버의 스키마에서 가져온 타입과 프론트 쪽의 tsx파일에 작성해둔 query 혹은 mutation으로 부터 생성된 타입들이 `graphql.ts` 파일에 위치하게 된다. 그리고 `gql.ts` 파일에 새로 생성된 `gql`함수를 통해 기존 아폴로클라이언트의 `gql` 함수를 통해 쿼리를 전달하는게 아니라 생성된`gql` 함수에 쿼리문을 매개변수로 전달하여 타입체크가 가능해진다.(이떄 두번쨰 매개변수로 variables를 바로 전달할 수도 있다.)

```
// gql 함수 사용할 때
import React from "react";
import { gql } from "@apollo/client";

const LOGIN_MUTATION = gql`
  mutation logIn($email: String!, $password: String!) {
    login(email: $email, password: $password) {
      ok
      error
      token
    }
  }
`;

// 생성된 gql함수 사용할 때 (쿼리문을 매개변수로 넣어줘야한다, 두번쨰 매개변수로 variables를 바로 전달할 수 도 있다.)
import React from "react";
import { gql } from "../__generated__/gql";

const LOGIN_MUTATION = gql(`
  mutation logIn($email: String!, $password: String!) {
    login(email: $email, password: $password) {
      ok
      error
      token
    }
  }
`,{variables:{email:'asdfa',password:'asdfadsf'}});
```