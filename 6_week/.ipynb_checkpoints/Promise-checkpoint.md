> 아래 코드는 자바스크립트에서 가장 악명 높은 **'콜백지옥'** 을 표현한 코드다.
```javascript
fs.readdir(source, function (err, files) {
  if (err) {
    console.log('Error finding files: ' + err)
  } else {
    files.forEach(function (filename, fileIndex) {
      console.log(filename)
      gm(source + filename).size(function (err, values) {
        if (err) {
          console.log('Error identifying file size: ' + err)
        } else {
          console.log(filename + ' : ' + values)
          aspect = (values.width / values.height)
          widths.forEach(function (width, widthIndex) {
            height = Math.round(width / aspect)
            console.log('resizing ' + filename + 'to ' + height + 'x' + height)
            this.resize(width, height).write(dest + 'w' + width + '_' + filename, function(err) {
              if (err) console.log('Error writing file: ' + err)
            })
          }.bind(this))
        }
      })
    })
  }
})
```
해당 코드는 [callbackhell 사이트](http://callbackhell.com)에서 가져왔다.

코드를 작성할 때 비동기 처리를 해야 할 경우가 간혹(사실 자주) 생기는데, 이 때 작성자가 의식의 흐름대로 연속적으로 콜백 함수를 사용하다보면 이렇게 알아보기 힘들 정도로 코드가 복잡해지는 것을 어렵지 않게 볼 수 있다.

이러한 시각적 복잡성을 해결하기 위해 es6부터 Promise라는 개념이 도입되었다.

```javascript
let p1 = new Promise(function(resolve, reject){ //true->resolve, false->reject 
    setTimeout(function() { console.log('hello p1'); resolve(); // ..............................성공했을 때, 
                          }, 1500); }); 
let p2 = new Promise(function(resolve, reject){ 
    setTimeout(function() { console.log('hello p2'); reject(); // ..................................실패했을 때 
                          }, 2300); }); 
Promise.all([p1, p2]).then(function() {
    console.log('pass') // ..........둘 다 성공해야 출력 
    }).catch(function() {
    console.log('failed') // ......실패가 있으면 출력 }); 
```

코드의 흐름은 다음과 같다.
1. new를 통해 Promise를 생성해준다.
2. Promise는 매개변수 resolve, reject를 갖는다.
3. 생성자 선언과 동시에 비동기 콜백이 각각 실행된다.
4. 코드가 실행되면 1.5초 뒤 ```hello p1```, 2.3초 뒤 ```hello p2```가 실행된다.
5. ```hello p2```는 reject일 때 실행된 것이므로 Promise.all-에서는 catch 메소드가 실행된다.

만약, 여기서 resolve가 실행될 시간이 reject보다 늦도록 코드를 수정하면 어떨까?

```javascript
let p1 = new Promise(function(resolve, reject){
    setTimeout(function() {
        console.log('hello p1');
        resolve(); // 성공했을 때
    }, 3300);
});

let p2 = new Promise(function(resolve, reject){
    setTimeout(function() {
        console.log('hello p2');
        reject(); // 실패했을 때
    }, 2300);
});

Promise.all([p2, p2]).then(function() {
    console.log('pass') // 둘 다 성공하면 출력
}).catch(function() {
    console.log('failed') // 하나라도 실패하면 출력
});
```

그렇다면 콘솔은 아래 순서로 보여줄 것이다.
1. ```hello p2```
2. ```failed```
3. ```hello p1```

Promise.all- 에서 catch 메소드는 reject가 나오는 순간 바로 동작하기 때문이다.

이렇게 Promise는 상대적으로 콜백 지옥을 읽기 쉬운 형태로 보여준다.

비동기처리 뿐만 아니라 코드 작성시 각각의 단위가 언제 확정될지 모르는 상황을 생각해보면, Promise를 응용할 때 이러한 시간 흐름을 유연하게 제어할 수 있다는 장점이 있다.

---
그러나 코드에서 볼 수 있듯이 Promise를 사용해도 콜백 함수가 그대로 노출되어 있기 때문에 함수로 둘러싼 형태라는 것은 동일하다. 즉, 기존의 복잡한 코드를 더 보기 쉽게 했지만 연결된 코드를 분리한 것(문단구분)에 그친다.

이러한 한계를 해결하기 위해 등장한 것이 지금 대세가 되어 주로 쓰이는 **Fetch with Async & Await**이다.

