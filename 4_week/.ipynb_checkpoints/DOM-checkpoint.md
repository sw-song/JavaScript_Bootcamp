## DOM이란?
브라우저가 제공하는 객체(BOM-Browser Object Model) 중 하나로
BOM의 가장 상위 객체인 Window의
하위 객체라고 볼 수 있습니다.
 
넓은 의미로
DOM은 웹브라우저가 
HTML 문서를 인식하는
방식을 말하며
객체 참조를 통해 이루어집니다.
 
DOM이 제공하는 기능은 다음과 같습니다.
 
1. C(create)
2. R(read)
3. U(update)
4. D(delete)
---

### HTML 접근 방식 - 1. getElement(s)
우선, 기본적인 HTML을 만들어주고,
```HTML
<html> 
    <head> 
    </head> 
    <body> 
        <div id='joo_js'></div>
    </body> 
</html>

```
JavaScript로 HTML을 조작하기 위해 DOM을 사용해 접근해본다.
```Javascript
let joo = document.getElementsByTagName('div');
```
여기서는 getElement(s)라는
복수 형태를 사용했다.
 
이렇게 만들어준
변수 joo는
배열 형태로 div를 가지고 있게 된다.
 
현재 코드에는
단 하나의 div만 존재하지만
div에 접근하기 위해서는
joo[0] 와 같이
배열 첫 번째 원소를
사용한다고
명시해주어야 한다.
 
만약 배열 인덱스를 이용하고 싶지 않다면
단수 형태로 변수 joo를 만들어주면 되겠다.
**단, 아래와 같은 단수형 문법은 제공되지 않는다. 에러가 날 것이다.**
```Javascript
let joo = document.getElementByTagName('div');
```

단수형으로 불러오기 위해서는 다른 방식을 사용해야 한다. 우리는 앞에서(HTML) div에 미리 id값을 지정해주었다. id값을 활용해 불러오는 방식은 아래와 같다.
```javascript
let joo = document.getElementById('joo_js');
```
이렇게 하나의 element만 특정해서
접근할 수 있다.
 
그러나 getElement는
Id를 지정해주지 않는 이상
클래스를 포함해
모든 태그를 복수형으로 받아와야 한다는
불편함이 있다.

---
### HTML 접근 방식 - 2. querySelector(All) 
우리는 그래서
getElement 방식 대신
querySelector라는
조금 더 편리한 방법을 사용한다.
```Javascript
let joo = document.querySelector('div');
```
이렇게 해주면
joo는 HTML에서
가장 첫 번째 div element를
입력받게 됩니다.
 
또한 querySelector를 사용하면
```
띄어쓰기(엘리먼트 하위),
>(자식 태그),
아이디 선택자(#),
클래스 선택자(.)
```
를 
통해 다양한 루트로
원하는 태그에 접근할 수 있다는
장점이 있습니다.

따라서
태그명이 유일한
사용자 정의 태그에 접근하거나
특정 깊이의 위치에 있는
하나의 태그에 접근하기에
매우 효율적이라는 것을
알 수 있습니다.
 
또한
getElements가 제공하는 배열 형태 역시
'querySelectorAll'을 통해
동일하게
사용할 수 있다.
```Javascript
let joo = document.querySelectorAll('div');
```

---
### DOM과 이벤트
웹이 점점 발전하면서
그저 HTML 문서를 보여주는 것이 아닌
사용자가 입력하고 브라우저가 반응하는
그런 형태의
UI가 필요해지게 되었다.
 
그러나 자바스크립트만으로는
그저 작성된 그대로
시간 순서대로 한 줄씩 읽어나갈 뿐
사용자의 입력을 기다리는 식의
동작은 불가능하다.
 
대신 브라우저는
자체적으로 **'Event'(사건, 이벤트)**를 제공한다.
**이벤트는 팝업을 띄우거나
입력을 받고 페이지를 전환하는 등의
기능**이다.

그리고 모든 DOM 객체는
이러한 이벤트를 동작시킬 수 있는
메소드를 지닌다.
 
자바스크립트는 바로 DOM 메소드를 통해
브라우저가 제공하는
이벤트를 제어할 수 있게 되는 것이다.

예를 들어
```HTML
<div>
  <form id="js_form">
    <button type='submit'>
      <span>Click</span>
    </button>
  </form>
</div>
```
이렇게 HTML에서
form을 만들어주고
click 하면 이벤트가 발생하는
코드를 작성하면,

<div>
  <form id="js_form">
    <button type='submit'>
      <span>Click</span>
    </button>
  </form>
</div>
 
이런 화면이 나온다.

click을 누르게 되면
브라우저에서는
'submit' 이벤트를
발생시키게 된다.
 
이때
DOM은
submit 이벤트를 제어할 수 있도록
기능을 제공하는데,
자바스크립트 코드를 통해
확인해보자.

```javascript
form = document.querySelector('#js_form');

function onSubmit(event){
    event.preventDefault()
    alert('hi')
};
form.addEventListener('submit',onSubmit);
```
먼저 ID Selector(#)를 이용해
HTML의 form을 가져온다.
 
그리고 ```form.addEventListener``` 메소드를 이용해
submit 이벤트를 동작하도록 하는데요,
 
우리는 자바스크립트를 통해서
이벤트를 변형시켜
페이지가 전환되지 않고
알림 창을 띄우도록 한 것이다.