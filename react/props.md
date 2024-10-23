Property의 줄임말로 컴포넌트 간 데이터를 전달할 때 사용하는 객체를 말한다. 이 객체는 태그형식의 컴포넌트에 사용되는 property를 모은 객체를 말한다고 보면 된다. 다만 중요한 것은 부모 컴포넌트에서 자식 컴포넌트로 일방적인 방향으로만 전달된다.

```jsx
function ParentComponent() {
  const name = "John";

  return (
    <div>
      <ChildComponent userName={name} />
    </div>
  );
}

function ChildComponent(props) {
  return <h1>Hello, {props.userName}!</h1>;
}
// 구조분해할당 이용시
function ChildComponent({userName}) {
  return <h1>Hello, {userName}!</h1>;
}
```
