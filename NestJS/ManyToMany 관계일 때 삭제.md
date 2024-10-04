typerom의 @ManyToMany 관계일 때 보통과 같이 delete 메서드를 이용해서 지우면 관계테이블의 restraint로 인해 에러가 난다. 관계테이블의 값들은 전부 not-null 제약이 걸려있고 이를 typeorm의 옵션을 통해서 바꿀 방법이 현재로선 없다. 그렇기 때문에 지우기 위해선 만족하는 조건의 엔터티를 findOne 혹은 find 등의 메서드로 데이터베이스에서 로드한 후에 remove나 softRemove 메서드를 통해서 삭제해야 한다.

```ts
async deleteRestaurant(id: number) {
  try {
    const restaurant = await this.restaurants.findOne({ where: { id } });
    await this.restaurants.softRemove(restaurant);
    return { ok: true };
  } catch (error) {
    console.log(error);
    return { ok: false, error: 'xxx' };
  }
}
```