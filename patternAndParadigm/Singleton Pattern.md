하나의 클래스에 오직 하나의 인스턴스만 존재하는 패턴

	 //literal의 경우
	 const obj1 = {a:32}
	 const obj2 = {b:32}
	 console.log(obj1 === obj2) // false

	//new Object의 경우
	class Singleton {
		constructor() {
			if(!Singleton.instance) {
				Singleton.instance = this // 여기서 this는 인스턴스를 지칭
			}
			return Singleton.instance
		}
		getInstance() {
			return this
		}
	}
	const a = new Singleton() // 새로운 인스턴스 생성
	const b = new Singleton() // 조건문에 따라 기존 인스턴스 반환
	console.log(a === b) // true