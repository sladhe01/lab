## margin collapsing
block의 border가 겹치면 큰 margin을 따라간다.

## flexbox
지켜야할 규칙3가지
1. 자식 element에 어떤 것도 적어서는 안된다. 부모 element에만 명시해야된다.
```html
<head>
	<style>
		body{
			display: flex;
		}
	</style>
</head>

<body>
	<div></div>
	<div></div>
</body>
// 자식인 div를 flexbox로 사용하려면 그 부모인 body에만 flex container임을 명시해주면 된다.
```
2. align-items는 cross axis(교차축, 기본적으로 수직)에 작용
3. justify-contents는 main axis(주축, 기본적으로 수평)에 작용

## position

- static : 기본값으로 기준점이 됨
- fixed : 화면상 항상 같은 곳에 위치
- relative : 처음에 위치했던곳(static이었을 떄 위치)을 기준으로 top, left, right, bottom  만큼 위치
- absolute : static이 아닌 가장 가까운 부모를 기준으로 top, left, right, bottom  만큼 위치
*부모 중에 대상이 없다면 가장 상위인 body를 기준으로 작동
- fixed나 absolute 값을 적용하면 그 element는 다른 레이어에 위치한다
```
	<style>
	#first {
		height: 150px;
		width: 150px;
		background-color: tomato;
		position: fixed;
		top: 5px;
	}
	#second {
		height: 200px;
		width: 200px;
		background-color: blueviolet;
	}
	</style>
</head>
<body>
	<div id="first"></div>
	<div id="second"></div>
</body>
```

<img alt="screenshot 240815220413.png" src="https://github.com/sladhe01/lab/blob/main/html,%20css/images/screenshot%20240815220413.png?raw=true" data-hpc="true" class="Box-sc-g0xbh4-0 kzRgrI">
적용 전
<img alt="screenshot 240815220514.png" src="https://github.com/sladhe01/lab/blob/main/html,%20css/images/screenshot%20240815220514.png?raw=true" data-hpc="true" class="Box-sc-g0xbh4-0 kzRgrI">
적용 후

## Combinators
- 모든 combinators에 적용하는 게 아니라 바로 밑 자식 태그에만 영향을 주려면 > 표시를 사용하면됨
  `div > span` div 바로 아래의 자식 span tag에만 적용하겠다는 뜻
- 특정 태그 바로 다음에 적용하려면 + 로 조합
  `div + span` div 바로 다음에 오는 span 태그에만 적용하겠다는 뜻
- 특정 태그의 뒤에 존재하는 형제인데 바로 뒤에 오지 않을 때는 ~로 조합
  `div ~ span` div 바로 다음에 오지는 않지만 형제인 span 태그에 적용하겠다는 뜻

## Pseudo Classes
특정 키워드 상태의 selector를 이용하여 특정 element만 선택하는 방법
- `tag명:pseudo-class`를 이용하여 특정 조건을 만족하는 element에만 스타일 적용 가능
  `div:nth-child(3n+1)` 3n+1번째 div에 적용된다는 뜻
  
### States
- :active 마우스로 클릭되어 선택된 상태
- :hover 마우스 커서가 위에 올려져있는 상태
- :focus 키보드로 선택된 상태
- :visited `<a>`에만 해당, 이미 클릭된 링크의 상태
- :focus-within focus상태의 자식을 가진 부모 element의 상태

## Pseudo Elements
elements의 특정 부분만을 선택하는 방법
- `tag::pseudo-element`와 같은 방법으로 특정부분에만 스타일 적용 가능
  `input::placeholder` input의 placeholder에만 스타일 적용
  `span::selection`드래그된 부분만 스타일 적용
  
## Attribute Selectors
- `tag[attribute]` 특정 attribute를 가진 태그에 스타일 적용
- `tag[attribute="something"]` 특정 attribute의 key 값을 가진 태그에 스타일 적용
-  `tag[attribute~="something"]` "something"이라는 속성값을 가지거나 "something"을 포함하여 공백으로 구분된 key를 가진 태그에 모두 적용
  `div[class~="name"]`클래스가 lastname, firstname, "name age"등의 태그에 모두 스타일 적용 하지만 "name-age"와 같이 하이픈으로 구분된 경우는 미적용
- `tag[attribute|="something"]` "something"으로 시작하지만 바로 뒤에 하이픈으로 구분되는 key값을 가진 태그에 스타일 적용
  `div[class|="round"]` 클래스 이름이 round-corner, round-font 등 round로 시작하는 태그에 모두 적용, "round around" 처럼 여러개의 클래스가 조합된 경우에는 적용 안됨
- `tag[attribute^="something"]` "something"으로 시작하는 key값을 가진 태그에 스타일 적용
  `div[class^="round"]` 클래스 이름이 round-corner, round-font 등 "round long"등 round 시작하는 태그에 모두 적용, "long round"처럼 시작이 다르면 적용 안됨
- `tag[attribute]$="something"` "something"으로 끝나는 key 값을 가진 태그에 스타일 적용
   `div[class$="round"]` 클래스 이름이 corner-round, "long round" 등 round 로 끝나는 태그에 모두 적용, "round long"처럼 마지막이 다르면 적용 안됨
- `tag[attribute]*="something"` "something"을 포함하는 key 값을 가진 태그에 스타일 적용
   `div[class*="round"]` 클래스 이름이 corner-round, "long round" , "round long" 등 round를 포함하면 모두 적용
   </br>
  *예외로 required의 경우 `tag:requried` 혹은 `tag:optional`로 적용

## Custom Properties(Variables)
css property의 value값을 변수와 같은 형식으로 저장할 수 있다.

1. `--`붙이고 변수명을 정해주면 된다.(띄어쓰기 불가능, 하이픈으로 연결)
   명시해준 태그가 그 변수가 적요되는 스코프 나타냄(global로 사용하려면 :root selector 이용)
```
   :root {
	   --main-color: #489ee3;
   }
```

2. @property 문법을 이용하여 변수 할당
```
@property{
	syntax: "<color>"; //변수가 적용될 태그 명시
	inherits: false; //상속 여부
	initial-value: #c0ffee; //초기값 지정, 참고로 변수에 다른 값 재할당 할 수 있음
}
```

3.  JS의 window.CSS.Property() 메서드 이용 2번과 사용법은 거의 동일
```
window.CSS.registerProperty({
  name: "--item-color",
  syntax: "<color>",
  inherits: false,
  initialValue: "aqua",
});
```

`var(변수명)` 로 변수 참조 가능
```
details {
  background-color: var(--main-bg-color);
}
```

## Transitions
state 변경될 때 애니메이션 효과를 주는 css property
state가 없는 selector쪽에 명시해줘야함
```
transition: property-name duration;
transition: property-name duration easing fucntion;
transition: property-name duration easing function delay;
transition: property-name duration behavior;
transition: property-name duration delay;
transition: property-name1 duration1, property-name2 duration2;
//easing function은 애니메이션이 어떻게 변하는 지를 나타냄
//1~5의 방식을 이용하여 ,를 이용해 여러개에 적용가능
//모든 대상에 적용시키려면 대상에 all 기입
```

## Transform
element를 변형시키는 css property
좌표공간 변경을 통해 변형시킴
다른 요소에는 영향을 미치지 않음 (margin, padding 처럼 다른 요소 밀어내지 않음)

<img alt="cartesian-coordinate-system-three-dimensional-diagram-codesweetly-1.png" src="https://github.com/sladhe01/lab/blob/main/html,%20css/images/cartesian-coordinate-system-three-dimensional-diagram-codesweetly-1.png?raw=true" data-hpc="true" class="Box-sc-g0xbh4-0 kzRgrI" width=50%>
좌표 축은 위와 같음

## Animation
transition과 달리 state 변경이 없이 animation 효과를 줄 때 사용하는 css property
1. animation 정의
   `@keyframes animation-name{from{} to{}}` 와 같은 문법으로 정의
```
<style>
	@keyframes flipp {
		0% {
			transform: rotateY(0);
		}
		50% {
			transform: rotateY(180deg) translateY(100px);
		}
		100%{
			transform:rotateY(0);
		}
	}
// 위와 같이 %로 표시된 진행도로 표시할 수도 있다
```
2. animation 적용
   `animation: keyframes duration easingfunction delay iteration-count direction fill-mode play-state name`과 같은 문법으로 적용
```
	img {
		animation: flipp 5s ease-in-out infinite;
	}
</style>
//문법상 몇몇 값은 빠질 수도 있음
```

## Media Queries
반응형 웹을 만들기 위해 사용자의 스크린 크기, 방향등의 조건에 맞춰 어떤 css를 적용할 지 분기를 나눌 수 있음