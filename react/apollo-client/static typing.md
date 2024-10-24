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

그다음으로 `npm run graphql:codegen` 명령어를 실행시켜주면 `__generated__`라는 디렉토리에 백엔드서버의 스키마에서 가져온 타입과 프론트 쪽의 tsx파일에 작성해둔 query 혹은 mutation으로 부터 생성된 타입들이 