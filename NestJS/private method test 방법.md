spyOn의 첫번째 매개변수로 사용되는 객체를 any타입으로 명시해주고 메서드에 접근할 때는 bracket notation 방식으로 접근한다.

```ts
class Service {
  ...
  private async sendEmail() {
  ...
  }
}

describe('sendVerificationEmail', () => {
  it('should call sendEmail', async () => {
	...
    jest.spyOn(service as any, 'sendEmail');
    expect(service['sendEmail']).toHaveBeenCalledTimes(1);
    ...
  });
});
```

혹은 아래와 같이 변수로 지정해서 쓰면 더 편하다.
```ts
class Service {
  ...
  private async sendEmail() {
  ...
  }
}

describe('sendVerificationEmail', () => {
  it('should call sendEmail', async () => {
	...
    const sendEmailSpy = jest.spyOn(service as any, 'sendEmail');
    expect(sendEmailSpy).toHaveBeenCalledTimes(1);
    ...
  });
});
```