spyOn을 이용하여 prototype 속성에서 private method를 당겨와서 쓸 수 있다.

```javascript
class OrderBuilder {
  public build() {
    this.handleError()
  }
  private handleError() {
    throw new Error('missing ... field in order')
  }
}

describe('Order Builder', () => {
  it('should test the handleError', () => {
    const handleErrorSpy = jest.spyOn(OrderBuilder.prototype as any, 'handleError');
    const orderBuilder = new OrderBuilder()
    expect(() => orderBuilder.build()).toThrow('missing ... field in order');  // Success!
    expect(handleErrorSpy).toHaveBeenCalled();  // Success!
  });
});
```