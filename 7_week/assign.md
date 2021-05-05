### 객체 원본의 수정 가능성
"객체는 복사되지 않고 **참조**된다." 라고 했다.

이러한 자바스크립트의 특징 때문에 예상하지 못한 값의 변화로 에러를 경험하기도 한다.

개발자들은 에러의 원인은 **상태**에 있다고 한다.

상태란, 변수가 가지는 값이 시간의 흐름에 따라 다를 수 있는데 그 때 **변하는 값**을 의미한다.

코드를 작성할 때 상태를 나타내야 한다면, 그리고 그 상태가 현실의 모습을 재현한 객체의 형태인 경우 코드가 실행되는 순간부터 지속적으로 참조하고 있는 객체 원본이 변경될 가능성을 예상할 수 있다.

사람은 현실이든 코드든 '원본'이 변하는 것을 좋아하지 않는다. 원본은 유일해야 하며 수정이 필요한 경우 복사본을 이용한다. 그래서 원본이 '원본'인 것이다.

그런 점에서, 객체를 참조하고 원본을 수정할 수 있는 동작은 자바스크립트의 '취약점'이다.
이 동작(객체 원본을 변형하는 동작)은 ```prototype```을 통해 너무 쉽게 열려있다.

---
### assign()
이러한 문제를 해결하기 위해 assign()이라는 메소드가 등장했다.

특정 객체를 전달할 때 원본을 참조하는 것이 아니라 새로 만들어서(복사) 전달하자는 컨셉이다.

사실 assign() 메소드가 등장하기 전까지는 주로 json을 활용했다.

객체를 json 형태로 변환시키고 다시 변환된 json을 새로운 객체로 저장하면서 같은 내용의 서로 다른 객체(다른 주소값을 가리키는)를 만드는 형태였다.

예를 들면,
```javascript
let car = { color:'red', price:500, fe:9 } 
let carJson = JSON.stringify(car) //obj를 json 문자열로 변환 
let newCar = JSON.parse(carJson) // json 문자열을 객체로 변환 
console.log(newCar) // { color: 'red', price: 500, fe: 9 }

```
그러나, 이렇게 json 변환을 거치는 복잡성은 코드가 길어지게 하고 객체의 복사가 자주 필요한 코드에서는 피로감을 줄 수 있다.

새로 등장한 ```assign()```은 이보다 더 편리하고 간결한 문법을 제공한다.

```javascript
let car = { color:'red', price:500, fe:9 } 
let newCar = Object.assign({}, car) // 새로운 객체 {}를 만들고 car 객체 복사 
console.log(newCar) // { color: 'red', price: 500, fe: 9 }
```

> The Object.assign() method is used to copy the values of all enumerable own properties 
from one or more source objects to a target object. It will return the target object.
-mozilla.org


코드 형태를 보면 ```Object.assign(target, ...sources))``` 와 같이 표현되어 있다.(코드명세)

assign은 다수의 parameters를 전달받는데, 첫번째는 만들고자 하는 ```target object```, 그 외에는 모두 복사하고자 하는 ```source objects```이다.

즉, 위 코드에서는 newCar로 새로이 만들고자 하는 객체 {}(빈객체 지정)를 target으로 설정해주고, source object로서 car 객체를 이용한 것이다.

---
### 객체 병합 현상
```assign()``` 메소드를 사용할 때 주의할 점은 '객체 병합' 현상이다.

만들고자 하는 target object를 위와 같이 빈 객체로 두지 않고 내용을 먼저 담은 경우,
그리고 source object에 이미 같은 속성이 포함되어 있다면 '덮어쓰기+'를 하게 된다.

예를 들면,
```javascript
const target = {a:1, b:2};
const source = {b:4, c:5};

const returnedTarget = Object.assign(target, source);

console.log(target);
```
```
Object { a: 1, b: 4, c: 5}
```
```javascript
console.log(returnedTarget);
```
```
Object { a: 1, b: 4, c: 5}
```

새로운 객체를 담을 변수 ```returnedTarget```은 target object를 통해 만들어진다.
그렇다는 것은 target(객체)은 returnedTarget과 같은 주소값을 공유한다는 것을 의미한다.

```assign()``` 메소드로 객체를 만들고 그 객체를 returnedTarget에 반환한 것이기 때문에 당연하게도 target을 확인해보면 ```{a: 1, b: 2}```가 아닌, ```{a: 1, b: 4, c: 5}```로 출력이 된다.

```assign()```은 객체의 원본 변형을 피하기 위한 목적으로 등장했다. 따라서 아래와 같이 target object는 빈 객체로 만들어주고 source objects를 넣어주는 형태로 코드를 작성해주는 것이 좋겠다.

```javascript
const target = {a:1, b:2};
const source = {b:4, c:5};

const returnedTarget = Object.assign({}, target, source);

console.log(target);
```
```
Object {a: 1, b: 2}
```
```javascript
console.log(returnedTarget);
```
```
Object {a: 1, b: 4, c: 5}
```

이렇게 ```assign()```을 활용함으로써 객체 원본을 유지하고 참조 관계를 완벽히 단절시킬 수 있게 되었다.

---
정리하면, 상태가 변하면 에러가 발생할 가능성이 증가하는데 특히 원본이 저장된 변수의 상태가 변하는 것은 코드에 치명적인 문제를 야기할 수 있다.

```assgin()```은 객체가 다른 변수에 넘겨질 때 참조가 아닌 복사가 되도록 한다. 따라서 ```assign()```을 활용하면 원본 변형의 문제를 해결할 수 있겠다.
