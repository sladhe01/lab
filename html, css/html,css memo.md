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