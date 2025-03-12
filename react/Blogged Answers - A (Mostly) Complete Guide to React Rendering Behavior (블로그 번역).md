# "렌더링"이란 무엇인가?
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
커밋 단계를 거쳐서 DOM을 업데이트하고 나면, React는 요청된 DOM 노드와 컴포넌트 인스턴스를 가리키도록 모든 참조사항들을 업데이트한다. 그 후에 `componentDidMount`와 `componentDidUpdate` 클래스 생명주기 메서드와 `useLayoutEffect` 훅을 동기적으로 실행한다.

그 후 React는 짧은 타임아웃을 세팅하고, 타임아웃이 끝나면 모든 `useEffect`훅을 실행한다. 이 단계는 "Passive Effect"라고도 알려져 있다.

>React 18부터 `useTransition`과 같은 "Concurrent Rendering(동시 렌더링)" 기능이 추가됐다. 이를 통해 React는 브라우저가 이벤트를 처리할 수 있도록 렌더 단계의 작업을 잠시 멈출 수 있다. React는 나중에 적절한 시점에 해당 작업을 재개하거나, 폐기하거나 또는 재계산할 수 있다. 렌더 패스(Render Pass)가 완료되면 React는 커밋 단계를 하나의 동기적인 단계로 실행할 것이다.

여기서 가장 핵심적인 부분은 "렌더링"과 "DOM을 업데이트하는 것"은 같은 것이 아니며, 어떤 컴포넌트는 결과적으로 시각적 변화 없이도 렌더링 될 수 있다.
- 그 컴포넌트는 가장 최근 렌더링 결과와 같은 결과물을 반환할 것이고, 그러므로 어떤 변화도 필요하지 않다.
- Concurrent Rendering에서 React가 컴포넌트를 여러번 렌더링할 수 있겠지만, 현재 진행중인 작업을 무효화 한다면 매번 그 렌더링 결과물을 버릴 수 있다. 

이 [React 생명 주기 다이어그램](https://julesblom.com/writing/react-hook-component-timeline)이 렌더링, 커밋, 훅 실행의 과정을 이해하는 데 도움을 줄 것이다.

추가적인 시각자료가 필요하다면 아래의 링크를 참고하자:
- [React hooks flow diagram](https://github.com/donavon/hook-flow)
- [React hooks render/commit phase diagram](https://wavez.github.io/react-hooks-lifecycle/)
- [React class lifecycle methods diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

# React는 어떻게 렌더를 다룰까?
### Queing Renders
최초 렌더가 끝난 후, React가 큐에 리렌더를 요청하는 몇가지 다른방법이 존재한다:
- 함수 컴포넌트:
    - `useState`의 setters 호출
    - `useReducer`의 dispatch 호출
- 클래스형 컴포넌트
    - `this.setState()` 호출
    - `this.forceUpdate()` 호출
- 그 외:
    - ReactDOM 최상위레벨의 `render(<App>)`메서드 재 호출(루트 컴포넌트에서 `forceUpdate()`를 호출하는것과 동일하다.)
    - `useSyncExternalStore`훅을 통해 트리거되는 업데이트
함수형 컴포넌트는 `forceUpdate`메서드가 없지만, `useReducer`훅의 카운터를 항상 증가시키는 방식으로 동일한 동작을 구현할 수 있다:
```jsx
const [, forceRender] = useReducer((c) => c + 1, 0);
```

### 표준적인 렌더 동작
>이것만은 꼭 기억해야 한다: 
>React는 기본적으로 부모 컴포넌트가 렌더링되면, 그 안의 모든 자식 컴포넌트를 재귀적으로 렌더링한다.

예를 들어, A > B > C > D 구조로 되어있는 컴포넌트 트리가 있고, 이것들이 이미 페이지에 다 보여지고 있다. 이 때 유저가 B 컴포넌트의 카운터를 증가시키는 버튼을 누르는 경우를 가정해 보자:
- B에서 `setState()`를 호출하고 B의 리렌더링을 예약한다.
- React는 트리의 맨 위부터 렌더링을 시작한다.
- React는 A의 업데이트가 필요없다는 표시를 보고 그냥 통과한다.
- React는 B의 업데이트가 필요하다는 표시를 보고 렌더링을 한다. B는 이전과 같이 `<C />`를 반환한다.
- React는 C의 업데이트가 필요하다는 표시는 없지만, B가 부모 컴포넌트이기 때문에 아래 단계의 C 또한 렌더링한다. C는 `<D />` 를 다시 반환한다.
- D도 마찬가지로 업데이트가 필요하다는 표시가 없지만 부모 컴포넌트인 C가 렌더링 되었기 때문에 React는 아래 단계의 D도 렌더링한다.

다시말하자면, 기본적으로 어떤 컴포넌트를 렌더링하면 그 컴포넌트안에 존재하는 모든 컴포넌트의 렌더링을 야기한다고 할 수 있다.

여기서 또다른 주목해야할 점은 점은 보통의 렌더링에서 React는 prop의 변경되었는지 신경도 쓰지 않는다는 것이다. 단지 부모가 렌더링 되었기 때문에 무조건적으로 자식도 렌더링 되는 것이다.

이는 즉, `<App>` 컴포넌트에서 `setState()`를 호출하면 컴포넌트 트리 안에 있는 모든 컴포넌트가 렌더링된다는 것을 의미한다. 결과적으로 React는 [매번 업데이트를 할 때마다 어플리케이션 전체를 다시 그리는 것](https://www.slideshare.net/floydophone/react-preso-v2)처럼 동작한다.

이제 컴포넌트 트리 안에 있는 대부분의 컴포넌트는 직전과 똑같은 렌더링 결과물을 반환할 가능성이 높다. 따라서 React는 DOM에 변화를 줄 필요가 없다. 하지만 React는 계속해서 컴포넌트에게 렌더링을 요청하고 그 결과물을 비교해야만 할 것이다. 이 두 작업은 모두 시간과 노력이 꽤 든다.

아무튼 기억해야할 점은, **렌더링은 절대로 나쁜게 아니라는 점이다. 렌더링은 React가 DOM에 변화를 줘야할지 여부를 파악하는 방법일 뿐이다.**

### React의 렌더링 규칙
렌더링은 "순수"해야만 하며 어떠한 사이드 이펙트를 가져서는 안된다는 React 렌더링의 핵심적 규칙 중 하나다.

이 말이 이해가 안되고 헷갈릴 수 있다. 왜냐하면 수 많은 사이드 이펙트가 눈에 띄지 않고 결과물을 망가뜨리거나 하지 않기 때문이다. 몇가지 예시를 들어보자, 엄밀히 봤을 때 `console.log()`구문은 사이드 이펙트다. 하지만 이 구문이 실질적으로 아무것도 고장내지 않는다. prop을 변경하는 것 역시 명백한 사이드 이펙트지만 이것 또한 아무것도 고장내지 않을 것이다. 렌더링 중간에 AJAX 호출을 하는 것 또한 명백한 사이드 이펙트고 요청의 유형에 따라 앱의 예기치 못한 동작을 야기할 수 있다.

Sebastian Markbage가 작성한 [[The Rules of React]]라는 훌륭한 문서가 있다. 여기서 그는 서로 다른 (렌더를 포함한)React 생명 주기 메서드의 예상되는 동작과, 어떤 작업이 안전하게 "pure"한 것으로 간주되는지, 어떤 작업이 안전하지 않게 간주되는지를 정의하고 있다. 문서 전체를 읽어볼 만한 가치가 있지만, 중요한 몇가지를 간추려 보겠다:
- 하면 안되는 Render 규칙
    - 이미 존재하는 변수나 객체를 변경하면 안된다.
    - `Math.raond()`또는 `Date.now()`와 같은 랜덤한 값을 생성하면 안된다.
    - 네트워크 요청을 만들면 안된다.
    - state 업데이트를 예약하면 안된다.
- 해도 괜찮은 Render 규칙
    - 렌더링 도중 생성된 객체를 변경하는 것은 괜찮다.
    - 에러를 throw하는 것은 괜찮다.
    - 캐싱된 값과 같이 아직 생성되지 않은 데이터를 "Lazy initialize"하는 것은 괜찮다.

### 컴포넌트 메타데이터와 Fibers
React는 어플리케이션에 존재하는 모든 컴포넌트 인스턴스를 추적하는 내부 자료 구조를 갖고 있다. 이 내부 구조의 가장 핵심적인 조각인 객체를 "fiber"라고 부른다. "fiber"는 다음과 같은 것들을 나타내는 메타데이터 필드를 가지고 있다:
- 특정 컴포넌트 트리의 시점에서 렌더링 해야 할 컴포넌트 타입
- 이 컴포넌트와 관련된 현재 prop와 state
- 부모, 형제, 자식 컴포넌트를 향한 포인터
- 렌더링 과정을 추적하기 위해 React가 사용하는 다른 내부 메타데이터
```ts
export type Fiber = {
  // Tag identifying the type of fiber.
  tag: WorkTag;

  // Unique identifier of this child.
  key: null | string;

  // The resolved function/class/ associated with this fiber.
  type: any;

  // Singly Linked List Tree Structure.
  child: Fiber | null;
  sibling: Fiber | null;
  index: number;

  // Input is the data coming into this fiber (arguments/props)
  pendingProps: any;
  memoizedProps: any; // The props used to create the output.

  // A queue of state updates and callbacks.
  updateQueue: Array<State | StateUpdaters>;

  // The state used to create the output
  memoizedState: any;

  // Dependencies (contexts, events) for this fiber, if any
  dependencies: Dependencies | null;
};
```
(React 18에 있는 Fiber의 타입에 대한 모든 정의는 [이곳](https://github.com/facebook/react/blob/v18.0.0/packages/react-reconciler/src/ReactInternalTypes.js#L64-L193)에서 볼 수 있다.)

"fiber"는 실제 컴포넌트의 props와 state 값들을 저장하고 있다는 것을 명심해야 한다. 컴포넌트의 props나 state를 사용할 때 React는 실제로 fiber 객체에 저장된 값에 접근하여 제공한다. 특히 클래스 컴포넌트의 경우 React는 컴포넌트를 렌더링하기 직전에`componentInstance.props = newProps`와 같이 명시적으로 컴포넌트로 복사한다. 그래서 `this.props`가 존재하는 것은 React가 내부 데이터 구조에서 참조를 복사해왔기 때문이다. 그런 의미에서 컴포넌트는 React의 fiber 객체의 일종의 외관인 것이다.
(실제로 props와 state는 fiber 객체 안에 저장되지만 컴포넌트는 그 값을 마치 자신의 것처럼 노출한다는 의미)

비슷하게 React 훅이 동작하는 이유는 React가 컴포넌트에서 쓰는 모든 훅들을 컴포넌트의 fiber객체와 연결된(attatched) 연결 리스트(linked list)형태로 저장하기 떄문이다. React가 함수형 컴포넌트를 렌더링할 때 fiber에서 해당 훅의 연결 리스트를 가져온다. 그리고 다른 훅을 호출할 때 마다, React는 fiber에 저장된 훅 정보 객체에서 해당 훅과 관련된 적절한 값을 반환한다.(예로 state나 `useReducer`에서 사용하는 dispatch 값을 들 수 있다.)

부모 컴포넌트가 자식 컴포넌트를 처음으로 렌더링할 때, React는 컴포넌틑의 "인스턴스"를 추적하기 위해 fiber 객체를 생성한다. 클래스형 컴포넌트는 `const instance = new YourComponentType(props)`를 명시적으로 호출하여 컴포넌트 인스턴스를 fiber 객체에 저장한다. 함수형 컴포넌트에서 React는 `YourComponentType(porps)`를 함수로 호출한다.

### 컴포넌트 타입과 재조정(Reconcilation)

