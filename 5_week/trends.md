# 자바스크립트 개발 트랜드

> 자바스크립트 개발자들은 자바스크립트의 높은 자유도를 장점보다는 단점으로 생각했고 좀 더 명시적인 코드를 원했다.

### 1. 가변 인자
자바스크립트의 함수는 인자를 적게 받거나 다른 타입으로 받더라도 에러를 반환하지 않는다.

자바스크립트 함수는 매개변수를 지정하지 않고 가변 인자(변할 수 있는 인자)를 받을 수 있도록 설계되었기 때문이다.

내부적으로 보면 모든 함수가 'arguments'라는 변수를 가지고 있다.

```javascript
function sum() {
    let res = 0;
    for (let i = 0; i < arguments.length; i++){
        res += arguments[i]; // 별도로 선언하지 않은 arguments를 사용하고 있다.
    }
    return res;
}
```
arguments는 모든 함수가 내부적으로 가지고 있는 변수로, 우리가 선언하지 않아도 동작한다.
console.log()를 통해 arguments를 확인해보자.

```javascript
function sum() {
    let res = 0;
    for (let i = 0; i < arguments.length; i++) {
        res += arguments[i];
    }
    console.log(arguments);
    return res;
}
sum(1,2,3,4)
```
```
[Arguments] {'0':1, '1':2, '2':3, '3':4}
```
arguments는 배열일 것 같지만 객체이다.
이러한 형태의 객체를 유사 배열이라고 한다.

이렇게, parameter가 별도로 정의되어 있지 않아 직관적이지 않다는 점과 배열 메소드를 사용할 수 없는 **유사 배열**로 동작한다는 점이 불편하게 느껴져 새로운 문법이 탄생하게 되었다.

새로운 문법이 적용된 코드는 아래와 같다.
```javascript
function sum(...args) {
    let res = 0;
    for (let i = 0, i < args.ength; i++) {
        res += args[i];
    }
    console.log(args);
    return res;
}
sum(1,2,3,4)
```
```
[1, 2, 3, 4]
```
...args를 parameter로 명시해줌으로써 **가변 인자**가 들어온다는 것을 직관적으로 확인할 수 있고, 유사배열이 아닌 **배열** 그대로의 모습으로 저장된다.

이렇게 명시적인 코드의 이점, 배열 메소드를 직접 사용 가능한 편리함으로 인해 내부적으로 구현된 arguments를 그대로 사용하기보다 ```...args```를 사용하는 방식을 선호하게 되었다.

### 2. class
자바스크립트에서 class를 지원하게 되면서 class를 통해 constructor를 명시할 수 있게 되었다.

또한 클래스는 new 연산자로 호출하지 않으면 그 즉시 에러가 발생한다는 점에서 명시적 코드를 요구하는 것을 알 수 있다.

함수와 비교해보면, 함수는 new 연산자 사용을 강제할 수 없지만 class는 반드시 new로 인스턴스를 생성해야만 동작한다.

아래에서 함수형 코드, 클래스형 코드를 비교했다.

1. 함수형

```javascript
function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
} // descriptiveCar 메소드는 상속받는 모든 객체에 필요하지는 않기 때문에 Car 객체의 prototype에 별도로 저장한다.

Car.prototype.descriptiveCar = function(){
    return `
        make : '${this.make}
        model : ${this.model}
        year : ${this.year}
    `
}

```
2. 클래스형

```javascript
class Car {
    constructor(make, model, year) {
        this.make = make;
        this.model = model;
        this.year = year;
    } // 생성자에 바로 저장되는 부분
    
    descriptiveCar() {
        return `
            make = ${this.make}
            model = ${this.model}
            year = ${this.year}
            `;
    } // 프로토타입에 저장되는 부분
}

```

2+. 클래스로 생성한 객체 상속
```javascript
class firstCar extends Car {
    constructor(make, model, year) {
        super(make, model, year);
    }
} // firstCar에서 별도로 필요로 하는 속성이 없기 때문에 constructor를 작성하지 않아도 되지만 명시하는 것이 권장된다.

class secondCar extends Car {
    constructor(make, model, year) {
        super(make, model, year);
    }
    descriptiveCar() {
        return 'this is my second Car'
    } // descriptiveCar 메소드를 오버라이딩하면, 즉 하위 스코프에서 새롭게 메소드를 지정해주면 상위 스코프는 참조하지 않는다.(덮어쓴다)
}
// secondCar는 descriptiveCar() 메소드를 직접 내장하지 않고 프로토타입을 참조하므로 다음과 같이 호출한다.
console.log(secondCar.prototype.descriptiveCar())
```

이렇게 ```extends```를 통해 상속 관계를 명시할 수 있고 파이썬과 같은 다른 객체 지향 언어에서 사용하는 super()를 통해 부모 클래스를 참조함을 보여줄 수 있다.

또한 생성자를 constructor로 명시하고 위와 같이 상속 관계를 보여줄 수 있게 되면서 기존의 __proto__와 prototype으로 작성된 난해한 코드가 개선되었다.

기존에 함수로 상속을 구현할 때 상위 함수의 protytpe을 직접 연결해줘야 했지만 class의 도입으로 extends를 사용하여 보다 편리하게 코드를 작성할 수 있게 되었다.

물론, 자바스크립트의 class는 다른 객체 지향 언어의 그것과 개념적 차이가 있기에 __proto__, prototype에 대한 이해를 반드시 필요로 한다.

### 3. 배열 순회
ES5부터 등장한 배열 순회 메소드는 아래와 같다.
- forEach
- map
- filter
- sort
- reduce
- every/some
- indexOf
- ...

그 중, 자주 비교되는 forEach와 map을 다뤄보도록 하자.

기존에 자바스크립트는 배열 내 데이터를 활용할 때 주로 for문을 사용했다.
하지만 조건문, 반복문과 같이 조건에 값(데이터)이 들어가는 형태는 좋은 코드가 아니다.
값을 잘못 입력하거나 값이 지정된 변수가 변하면 코드는 동작하지만 잘못된 결과가 나올 수 있다.(이런 부분이 디버깅을 힘들게 한다.)
따라서 항상 코드를 작성할 때에는 데이터를 분리시켜야 한다.

예를 들면,
```javascript
let car = [
    {make : 'audi', model : 'a4'},
    {make : 'bmw', model : 'm4'},
    {make : 'kia', model : 'k5'}
];

for(let i=0; i<car.length; i++) {
    console.log(`${i+1}. make : ${car[i].make} / model : ${car[i].model}`)
}
```
```
1. make : audi / model : a4
2. make : bmw / model : m4
3. make : kia / model : k5
```

이런 형태를 '명령형 패러다임'이라 부른다.

조건을 매번 확인하고 실행하는 방식으로, 변수 i가 1씩 증가하며 계속 교체된다.

예상할 수 있듯이 이러한 코드에서는 인덱스를 잘못 잡거나 변수에 초기값이 잘못 들어간다면 에러가 나거나(over index 등) 에러 없이 잘못된 결과값을 반환하기도 한다.

forEach() 메소드는 이 점을 확실하게 해결해준다.

```javascript
let car = [
    {make : 'audi', model : 'a4'},
    {make : 'bmw', model : 'm4'},
    {make : 'kia', model : 'k5'}
];

/*
기존 코드 :
for(let i=0; i<car.length; i++) {
    console.log(`${i+1}. make : ${car[i].make} / model : ${car[i].model}`)
}
*/

car.forEach(function(eachCar, idx) {
    console.log(`${idx+1}. make : ${eachCar.make} / model : ${eachCar.model}`)
})
```
forEach()는 전달인자로 함수를 받는다.
이 함수는 콜백 함수로 세 인자를 받아 호출된다.

첫번째 인자로 배열 원소의 값(currentValue)을,
두번째 인자로 각 원소의 인덱스(index)를,
세번째 인자로 배열(array) 자체를 받는다.

보통 데이터인 배열 원소에 접근할 목적으로 forEach()를 사용하기 때문에 첫번째 인자(currentValue)만 기재하는 경우가 많다.
여기서는 두번째 인자, 인덱스까지 넣어주었다.

그리고 forEach()는 각 요소에 접근할 때마다 콜백함수를 호출하므로 여기서는 요소가 3개이므로 총 3번 함수를 호출하게 된다.

콘솔을 찍어보면 앞서 봤던 for문 방식과 정확히 동일한 일을 하는 것을 볼 수 있다.
```
1. make : audi / model : a4
2. make : bmw / model : m4
3. make : kia / model : k5
```

다음으로 map을 확인해보겠다.

map은 forEach와 같이 첫번째 전달인자로 함수를 받고, 그 함수 역시 동일하게 currentValue, index, array 순서의 인자를 가지고 호출되는 콜백 함수다.

forEach와 다른 점은 콜백함수의 결과(return)값들로 구성된 새로운 배열을 반환한다는 것이다.

코드를 보면 아래와 같다.
```javascript
let car = [ 
    {make : 'audi', model : 'a4'}, 
    {make : 'bmw', model : 'm4'}, 
    {make : 'kia', model : 'k5'} ]; 
    
/* 
기존 방식 1. for문:
for(let i=0; i<car.length; i++) { 
    console.log(`${i+1}. make : ${car[i].make} / model : ${car[i].model}`) 
    } 
            
*/ 
/*
기존방식 2. forEach()
car.forEach(function(eachCar, idx) {
    console.log(`${idx+1}. make : ${eachCar.make} / model : ${eachCar.model}`); 
    }); 
*/ 

cars = car.map(function(eachCar, idx){
    return `${idx+1}. make : ${eachCar.make} / model : ${eachCar.model}`; 
    }) 
console.log(cars)
```

forEach()를 사용할 때와 형태는 유사하지만 return을 해준다는 점이 눈에 띈다.

앞서 언급한 것처럼 map의 기능은 return된 값들로 새로운 배열을 생성해주기 때문이다.

콘솔 로그를 찍어보면,
```
[1. make : audi / model : a4
2. make : bmw / model : m4
3. make : kia / model : k5]
```
배열로 묶여있는 것을 볼 수 있다.

---
정리하면, 자바스크립트는 개발 특유의 오픈소스 문화가 활발해지고 협업의 가치가 높아지면서 더 명시적인 코드, 더 단순하고 명료한 코드로 진화해왔다.

자바스크립트뿐만 아니라 프로그래밍 언어는 사용자들간에 많은 논의를 통해 해마다 수정과 보안을 거치며 필요하다면 새로운 문법이 생겨날 수 있는데, 모든 방향은 '코드를 어떻게 잘 작성하고, 어떻게 잘 이해할 수 있을까?' 에 대한 고민에서부터 시작한다.
