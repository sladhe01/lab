# Fragment Masking

fragment masking은 프래그먼트에 포함된 필드만을 특정 컴포넌트에 접근할 수 있도록 제한하는 기능으로 이 기능을 이용함으로써 데이터의 leaking없이 데이터의 트리를 정확하게 컴포넌트에 전달할 수 있다. 
프래그먼트 타입을 이용하는 방법은 `...fragmentName`으로 필드를 기재하는 대신 프래그먼트로 대체하면된다. 
```ts
//fragments.ts(fragment 타입이 정의되는 파일)
import { gql } from "./__generated__";

export const RESTAURANT_FRAGMENT = gql(`
	fragment RestaurantParts on Restaurant {
		id
		name
		coverImg
		category {
			name
			}
		address
		isPromoted
	}
`);
```

```ts
//restaurants.tsx(프래그먼트를 이용하는 파일)
import React, { useState } from "react";
import { gql, getFragmentData } from "../../__generated__";
import { Restaurant } from "../../components/restaurant";
import { Category } from "../../components/category";
import { useForm } from "react-hook-form";
import { useHistory } from "react-router-dom";
import { Helmet } from "react-helmet-async";
import { RESTAURANT_FRAGMENT } from "../../fragments";
import { RestaurantPartsFragment } from "../../__generated__/graphql";

//...프래그먼트타입이름 으로 필드를 대체하면된다.
const RESTAURANTS_QUERY = gql(`
	query restaurantsPage($page:Int) {
		restaurants (page:$page) {
			ok
			error
			totalPages
			totalResults
			results {
				...RestaurantParts
			}
		}
	}
`);
```

## unmasking
타입스크립트는 useQuery()의 반환값 중에 하나인 data에서 fragment부분이 어떤 타입인지 masking되어 있어 알지 못하기 때문에 프래그먼트 내부의 데이터를 이용하는데 문제가 있다. 이를 해결하는 방법에는 두가지 방법이 있다.
### embrace fragment masking( useFragment( ) )
첫번쨰 방법으로 leaking을 방지할 수 있기 때문에 이방식을 가장 많이 추천한다.
useFragment는 명칭이 훅으로 오인하게 만들지만 그냥 함수다. 이 함수를 통해서 내부 타입이 명시된 fragment 객체를 반환한다.
`useFragment()`의 첫 번째 매개변수로는 unmasking할 프래그먼트를, 두 번째 매개변수로는 실제 unmasking 대상인 프래그먼트 타입을 가진 데이터 객체를 넣어주면 된다.

```ts
//restaurants.tsx
import { RESTAURANT_FRAGMENT } from "../../fragments";
...
export const Restaurants = () => {
	const { data, loading } = useQuery(RESTAURANTS_QUERY, {
		variables: { page } 
		}
	);
	const results = getFragmentData(
		RESTAURANT_FRAGMENT,
		data?.restaurants.results
	);
}

```

### Disalbe fragment masking
단순하게 codegen에서 타입을 생성할 때 fragment masking을 사용하지 않도록 설정해주면 된다.
```ts
//codegen.ts
  generates: {
    "src/common/graphql/types/": {
      preset: "client",
+     presetConfig: {
+       fragmentMasking: false,
+     },
```