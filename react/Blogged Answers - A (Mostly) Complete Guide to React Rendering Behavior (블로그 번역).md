## "렌더링"이란 무엇인가?
**렌더링**이란 React가 컴포넌트에게  현재 Props와 State에 기반하여 UI에서 어떻게 보여지고 싶은지 알려달라고 요청하는 과정이다.

### 전체적인 렌더링 과정
렌더링 과정동안, React는 컴포넌트 트리의 루트에서부터 시작하여 아래쪽으로 순환하며 업데이트가 필요하다고 표시된 컴포넌트를 전부 찾는다. 표시된 각각의 컴포넌트에 대해서, React는 클래스형 컴포넌트일 경우 `classComponentInstance.render()`또는 각 함수형 컴포넌트일 경우 FunctionComponent()`를 호출하고, 렌더결과물을 저장한다.

컴포넌트의 렌더 결과물은 보통 JSX구문으로 작성되며, JS가 컴파일되고 배포 준비가 되는 시점에서 `React.createElement()`호출로 변환된다. `createElement`는 일반적인 JS 객체 형식의 React 엘리먼트를 반환하는데, 이 엘리먼트는 생성하고자 하는 UI 구조를 설명한다.
```jsx
// 다음과 같은 JSX 문법이:
return <SomeComponent a={42} b="testing">Text here</SomeComponent>

// 이런 식의 호출로 변환됩니다:
return React.createElement(SomeComponent, {a: 42, b: "testing"}, "Text Here")

// 그렇게해서 이런 엘리먼트 객체가 됩니다:
{type: SomeComponent, props: {a: 42, b: "testing"}, children: ["Text Here"]}
```
전체 컴포넌트 트리에서 렌더 결과물을 모두 수집하고 나서, React는 새로운 객체 트리 ("가상 DOM"으로 자주 불리는 객체)와 비교할 것이다. 그러고는 의도한대로 보여지기 위해 실제 DOM에 적용시켜야 할 모든 변경 사항 목록을 수집한다. 이러한 비교 및 계산과정을 **재조정 (Reconcilation)** 이라고 한다.

그러고 나서 React는 이렇게 계산된 모든 변경사항을 하나의 동기적 시퀀스(synchronous sequence)로 실제 DOM에 적용시킨다.