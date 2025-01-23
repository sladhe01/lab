nestJS에서 환경변수를 이용하기 위해서는 dotenv 라이브러리를 이용하거나 dotenv를 내부적으로 이용하는  @nestjs/config 라이브러리를 이용해야 한다.
dotenv를 이용할 경우 main.ts에서 bootstrap을 호출전에 dotenv.config를 통해서 .env파일의 경로를 포함한 환경 설정을 해준다.
후자의 경우 app.module.ts의 @Module 데커레이터의 옵션인 imports에ConfigModule.forRoot를 통해서 환경설정을 한 모듈을 추가해준다.(isGlobal: true로 설정하면 다른 모듈에서도 imports에 기입하지 않고 환경변수를 이용할 수 있다.)
위 방법들을 통해서 환경변수를 이용할 수 있지만 app.module.ts를 제외한 module.ts파일에서는 module클래스 외부에서 환경변수에 접근하려고 하면 접근을 할 수 없다.
dotenv를 이용하는 경우던 ConfigModule을 이용하는 경우던 먼저 환경변수를 로드한 후에 module 클래스 외부의 코드를 로드해야 하는데 그 순서를 명시적으로 확정할 수 없기 때문인것으로 추측한다. 이제 겪었던 실제 문제를 통해서 해결방법을 제시하겠다.
```ts
import { Global, Module } from '@nestjs/common';
import { PUB_SUB } from './common.constants';
import { RedisPubSub } from 'graphql-redis-subscriptions';
import Redis from 'ioredis';

@Global()
@Module({
  providers: [
    {
      provide: PUB_SUB,
      useFactory: async () => {
        const options = {
          host: process.env.REDIS_HOST,
port: +process.env.REDIS_PORT,
retryStrategy: (times: number) => Math.min(times * 50, 2000),
};
return new RedisPubSub({
publisher: new Redis(options),
subscriber: new Redis(options),
});
},
},
],
exports: [PUB_SUB],
})
export class CommonModule {}
```