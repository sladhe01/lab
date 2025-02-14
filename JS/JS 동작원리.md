동작원리에 대해서 설명하기에 앞서 도식화된 동작 구조와 대략적인 개념을 알아보자.
<img style="display: block;-webkit-user-select: none;margin: auto;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;" src="https://raw.githubusercontent.com/sladhe01/lab/refs/heads/main/images/1_eFvdHDgxCM6C20Zt80I7Jg.webp">
기본적으로 자바스크립트는 싱글 스레드의 동기적 언어로 하나의 스택을 가지고 한번에 하나의 코드를 실행한다.
- Memory Heap : 메모리 할당이 일어나는 곳 (변수, 함수 등의 객체들이 담긴다)
  \* 자료구조중 하나인 Heap과는 별개의 용어
- Call Stack : 실행할 코드가 한줄 씩 쌓아서 실행하는 공간으로 함수의 실행이 끝나면 해당 함수는 스택에서 제거된다.
- Web APIs : 비동기 처리를 담당한다. Node.js에서는 백그라운드가 담당한다.
  \* JS엔진이 직접 멀티 스레드를 쓰는 건 아니고 브라우저나 Node.js가 대신 멀티 스레드를 활용하는 것이다.
- Queue : 비동기 처리가 끝난 후 실행되어야 할 콜백 함수가 차례대로 할당된다. (바로 실행되지 않는 코드의 대기실 개념)
- Event Loop : Queue에 할당된 함수를 순서에 맞춰 Call Stack에 할당해준다.