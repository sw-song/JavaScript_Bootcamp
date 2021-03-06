
### 1. 코드 중간중간에 데이터를 넣지 마라.
 코드는 읽는 시간이 작성하는 시간보다 훨씬 길다. 따라서 읽고 이해하기 쉽게 데이터는 데이터대로 따로 배치하는 것이 좋다. 그래서 간단한 수식 하나라도 변수를 통해, 네이밍을 해주는 것이 중요하다.
 
 처음 코딩에 입문할 때 변수를 따로 선언해주는 것이 더 복잡하게 느껴지고 코드의 길이도 길어지는 것 같아 의식의 흐름대로 작성하는 경우가 많은데, 그렇게 되면 추후에 수정하거나 변경할 일이 생기면 알아보기도 어려울뿐더러 수정은 더더욱 어렵다. 그러니 변수 선언과 적절한 네이밍은 습관처럼 연습하자.


### 2. if, switch문을 남발하지 말자.
 코드에 대한 정확한 이해 없이 코딩 테스트를 준비하거나 알고리즘 공부를 해본 경우, if문을 남발해본 경우가 있을 것이다. 이것 역시 의식의 흐름대로 코드를 작성한 경우인데, 함수를 정해두고 필요한 때에 적절히 사용하는 것과 처음부터 끝까지 조건문으로 작성하는 경우는 코드의 질이 다를 수밖에 없다. 자바스크립트는 함수의 활용 능력에 따라 역량이 달라진다는 것을 명심하자. 그리고 조건문에서는 항상 데이터가 코드 내부에 조건으로 따라붙는데, 이는 앞 단락에서 언급한 이유로 역시 사용을 최소화하는 것이 좋다.

### 3. 중복을 제거하자.
 중복 제거는 사실 코드 리팩토링에서 가장 첫 번째로 고려해야 할 사항이다. 중복된 코드를 변수에 할당해주고, 코드 전체의 길이를 줄여주는 것은 가독성을 높이는 데에 이점을 주기 때문에 반드시 중복을 끝까지 찾아내서 적절하게 변환해줘야 한다.

### 4. 함수와 객체를 이해하자.
 현대적 자바스크립트 코딩 방식은 문'보다 '식'을 주로 쓰는 스타일을 선호하기 때문에 습관적으로 조건문, 반복문부터 사용할 것이 아니라 함수와 객체를 잘 사용하기 위해 노력하는 것이 권장된다. 
 자바스크립트에서 데이터는 각각 하나의 값이며, 그러한 데이터 중 원시 데이터 타입을 제외하면 모든 데이터 타입이 객체다. 즉 배열, 함수, 정규표현식 등은 모두 객체로 표현되며 동시에 그 자체로 값이다.
 함수가 값이라는 것은 항상 호출에 실패하지 않으며, 특정 값을 반환한다는 것을 말한다. 또한 인수 처리가 느슨한 자바스크립트의 특성상 주어진 인수의 수가 맞지 않아도 함수 호출에 실패하지 않는다. 그러한 특징 때문에 함수를 사용할 때 인자 값 검증 역시 매우 중요하다.
 
 또한 함수는 '문'으로도 만들 수 있는데, 변수에 할당하지 않고 이름을 가지고 있으며 function example(){--return A}와 같이 선언만 해주는 형태이다. 이렇게 선언된 함수는 코드에서 값으로 사용된다. 이와 달리 함수 표현'식'은 재귀 함수 형태가 아닌 이상 이름을 가질 필요는 적다. 변수에 이름을 지정해서 사용하면 되기 때문이다.
 마지막으로 자바스크립트의 함수 트렌드는 기호를 사용하는 화살표 함수(한 줄 함수)이므로 꼭 연습하자.


### 5. 객체지향 프로그래밍과 캡슐화
 함수형 언어이자 객체지향 언어인 자바스크립트는 내부적으로 코드는 한 줄씩 읽어나가겠지만 사람은 그렇게 기계적으로 한 줄씩 읽을 수가 없다. 따라서 항상 코드는 이해하기 쉽도록 작성해야 하고 그때 필요한 것이 캡슐화다. 특정 함수 혹은 객체가 내부적으로 어떻게 구현되어 있는지 상세히 찾아보지 않아도 적절한 네이밍과 필요한 인자만 알기 쉽게 보여준다면  엄청난 양의 코드를 이해하는 데에 시간을 덜 쏟아도 될 것이다. 다만 자바스크립트가 인자 입력을 매우 유연하게 받는 특징 때문에 사용하기는 쉽지만 에러의 원인을 감지하기가 어려웠는데, ES6부터는 function sum(a, b... args)와 같은 방식으로 함수를 선언할 수 있게 되었다.
 
 코드를 작성하는 것보다 읽고 이해하는 시간이 훨씬 오래 걸리기 때문에 코드를 몇 글자 더 작성하는 것이 피곤하다고 느끼지 말고 어떻게 하면 더 명시적으로 이해하기 쉽게 작성할 수 있을까? 하는 고민을 항상 하도록 하자.

### 6. 세미콜론(;) 작성하기
 자바스크립트는 모든 '식'의 끝에 세미콜론(;)을 붙여주기로 약속되어 있다. 물론 자바스크립트는 내부적으로 세미콜론(;)이 없어도 '대부분' 오류 없이 해석할 수 있다. 단, '대부분'이라는 것은 100%는 아니라는 것을 말한다. 언제나 코드는 티끌 하나 차이 없이 완벽해야 한다. 그래야만 어떤 상황에서든 똑같은 결과를 보여주기 때문이다.
 
 세미콜론(;)을 빼먹는 실수 때문에 혹시라도 원하는 결괏값이 나오지 않는다면 그 작은 오류를 찾는데에 수많은 시간을 써야만 할 것이다. 귀찮다고 생각하지 말고 아주 작은 양의 코드를 작성할 때에도 항상 세미콜론(;)을 붙이는 습관을 들이자. 글에도 문장이 끝났다는 것을 보여주는 마침표(.)가 있듯이 자바스크립트에서 하나의 실행 단위를 보여주는 '식'에도 세미콜론(;)을 붙여줘야 안정적이고, 보기 좋은 코드가 아니겠는가!

