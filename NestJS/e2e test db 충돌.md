두번째 e2e test파일을 만들고 테스트를 시작하자 ```
```
ERROR [TypeOrmModule] Unable to connect to the database. Retrying (1)...
QueryFailedError: duplicate key value violates unique constraint "pg_class_relname_nsp_index"
```
이런 에러메시지를 나타냈고 두가지 테스트가 비동기적으로 일어나는데 하나의 데이터베이스를 가지고 테스트를 진행하기 떄문에 중복된 테이블을 생성 또는 삭제하면서 일어난 오류라고 추측했고 실행 스크립트에 `--runInband`를 추가해주자 해결되었다.
