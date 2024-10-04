아래와 같이 core에서 기본으로 제공하는 APP_GURAD를 이용하고 전역으로 제공할 가드를 useClass로 사용하면 된다.
```ts
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';
import { AuthGuard } from './auth.guard';

@Module({ providers: [{ provide: APP_GUARD, useClass: AuthGuard }] })

export class AuthModule {}
```