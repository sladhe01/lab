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

## Position
- top, right ,left, bottom 중 하나의 css property를 주면 기존 다른 요소들과 다른 레이어에 위치하게 된다.
```
	<style>  
```