typeorm 버전이 0.3.x 부터 더 이상 @EntityRepository 데커레이터를 지원하지 않기 때문에 다른 방식을 이용해야 한다.

```ts
import { Repository, DataSource } from 'typeorm';
import { Category } from '../entities/category.entity';
import { Injectable } from '@nestjs/common';

@Injectable()
export class CategoryRepository extends Repository<Category> {
  constructor(private dataSource: DataSource) {
    super(Category, dataSource.createEntityManager());
  }
}
```

NestJS는 생성자에 입력된 매개변수의 타입을 보고 그 타입과 동일한 인스턴스를 자동으로 생성하고(처음 사용되는 타입의 경우) 주입한다. 기본적으로 하나의 인스턴스가 생성되면 같은 타입의 의존성 주입에는 이미 생성된 인스턴스가 재사용된다. 즉, 위의 경우 `dataSource` 가 기본 데이터 베이스 연결과 별개의 새로운 연결이 아니라 기존 연결 그대로이다.

다시 정리하자면 NestJS의 의존성 주입 시스템 덕분에  클래스에서 `DataSource`를 생성자 매개변수로 선언하기만 하면, NestJS가 적절한 인스턴스를 자동으로 주입한다.

또 `extends Repository<Category>`를 통해 `CategoryRepository`가 부모 클래스의 메서드를 사용할 수 있지만, 이 메서드들이 실제로 호출되기 위해서는 `super()`로 초기화된 인스턴스가 필요하다. 즉, 메서드는 상속되어 사용 가능하지만, 이를 사용하기 위해서는 올바르게 초기화되어야 하기때문에 super()를 통해 부모 클래스인 `Repository<Category>` 의 생성자를 (target, entityManager 매개변수와 함께) 호출하여 `Repository<Category>` 의 메서드들을 온전히 사용할 수 있게 한다.