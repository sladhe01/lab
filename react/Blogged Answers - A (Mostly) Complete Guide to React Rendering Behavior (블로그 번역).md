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

그 후에 React는 이렇게 계산된 모든 변경사항을 하나의 동기적 시퀀스(synchronous sequence)로 실제 DOM에 적용시킨다.

> Note : 최근에 React 팀은 "가상 DOM"이라는 용어가 그리 대단한게 아니라고 밝혔다. Dan Abramov는 최근에 다음과 같이 말했다.
> _저는 "가상 DOM"이라는 용어를 폐기했으면 합니다. 이 용어는 2013년에는 말이 됐습니다. 왜냐하면 그때는 사람들이 React가 매번 렌더 할 때 마다 DOM 노드를 생성한다고 가정했기 때문입니다. 하지만 최근에는 이렇게 가정하는 사람이 거의 없습니다. "가상 DOM"은 마치 무슨 DOM 관련 이슈에 대한 임시방편 (Workaround)인 것 처럼 들립니다. 하지만 React는 그런게 아니에요.  
> React는 "value UI"입니다. React의 핵심 원칙은 UI는 문자열이나 배열처럼 그저 값 (value)이라는 겁니다. 여러분은 이 값을 변수에 저장하고, 어디든지 전달할 수 있으며, JavaScript의 제어 흐름 (Control Flow) 등등을 사용할 수 있습니다. 이러한 표현력 (expressiveness)이 핵심입니다. 변경 사항을 DOM에 적용하는 걸 막기 위한 비교 행위같은게 아닙니다.
> 심지어 React는 항상 DOM을 대표하지도 않습니다. 예를 들어 `<Message recipientId={10} />` 같은 것은 DOM이 아닙니다. 개념적으로 React는 `Message.bind(null, { recipientId: 10 })`와 같은 게으른 함수 (Lazy Function) 호출을 대표합니다._

### 렌더와 커밋 단계
React 팀은  이 과정을 개념적으로 크게 2가지 단계로 나눴다.
- 렌더 단계 : 컴포넌트를 렌더링하고 변경 사항을 변경 사항을 계산하는 모든 과정이 이루어지는 단계
- 커밋 단계 : 변경 사항을 실제 DOM에 적용하는 단계
커밋 단계를 거쳐서 DOM을 업데이트하고 나면, React는 요청된 DOM 노드와 컴포넌트 인스턴스를 가리키도록 모든 참조사항들을 업데이트한다. 그 후에 `componentDidMount`와 `componentDidUpdate` 클래스 생명주기 메소드 또는 `useLayoutEffect` 훅을 동기적으로 실행한다.