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