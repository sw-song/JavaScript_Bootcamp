# 문법과 데이터타입
> JavaScript의 문법은 크게 '식', '문'으로 나눌 수 있다.

---
### 식(Expression)
- '값'을 의미
- ex. 1+1(2, 계산값), 2>1(true, boolean), array(리터럴, 참조값), function()(함수, 호출값)
- '값'으로 정의된 모든 데이터는 **변수**에 넣을 수 있다.
- 따라서 함수와 배열 역시 특정 변수에 담을 수 있다.
- 또한, 함수는 '식'이기 때문에 항상 '값'을 반환하게 되는데, 이 때 우리는 'return'을 사용해서 함수를 통해 호출하고자 하는 값을 지정할 수 있다. 


### 문(Statement)
- '식'을 이루는 방식을 지시, 제어
- ex1. if(조건문), while(반복문)
- ex2. var a = 1(대입연산, 값의 이동으로 1이라는 '값'을 a라는 변수에 할당하도록 **지시**)
- if, while, for '문'은 실행의 흐름을 제어할 뿐, 어떤 값을 스스로 반환하지 않기 때문에 'return'을 사용할 수 없다.
- 따라서 '문' 내부에서 '식'을 활용해 값을 할당하는 방식으로 내보낸다.
- 하나의 '문'이 끝날 때 마다 세미콜론(;)을 표시해주어야 한다.
- 인터프리터 언어가 한 줄씩 실행한다는 점에서 세미콜론(;)은 자바스크립트가 인터프리터 언어라는 것을 잘 표현하고 있다.