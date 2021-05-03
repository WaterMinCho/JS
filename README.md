<!-- prettier-ignore -->
# **`JavaScript`**

Study for javascript by WaterMincho.<br>
민초의 자바공부.<br><br>

README는 나중에 위키나 개인 블로그로 정리 하기 전 일괄기록이기 때문에 일단 최신 순으로 정리하였다.

---

<br>

## **틱택토 컴퓨터전**

### `2021.05.03 tictactoe-self.html`<br><br>

### **플로우**

<p align="center"><img src = https://user-images.githubusercontent.com/74204327/116888857-e85d4380-ac66-11eb-92b0-1893ad4ddef8.png height = 350px width = 430px></p><br>

- 컴퓨터가 한번 더 진행해서 컴퓨터의 턴인지 물어보고 승패여부 따지는 식으로 반복

### **배운 개념**

- filter(()=>) : 조건()에 맞는 데이터 필터링
  - 빈칸인 데이터를 찾아서 그 데이터 길이만큼 반복하여 랜덤함수로 랜덤값 추출할 수 있게 한다.

---

<br>

## **틱택토**

### `2021.05.03 tictactoe.html`<br><br>

### **플로우**

- 3x3 표 위에서 삼목과 같은 룰로 진행하는 게임이다. 자바스크립트에선 이차원 배열로 표현한다.
<p align="center"><img src = https://user-images.githubusercontent.com/74204327/116865069-e635bd80-ac43-11eb-846f-d34b6f1dfc49.png height = 350px width = 430px></p>

### **배운 개념**

- ### **구조분해 할당**

  `배열`이나 `객체`의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식.
  속성명과 변수명이 일치하면 그 갯수가 어떻든 여러 개 표현 가능하다.

  ```javascript
  //객체 구조 분해
  const { body, createElement } = document;
  // 아래와 같다.
  // const body = document.body
  // const createElement = document.createElement

  const o = { p: 30, q: true };
  const { p, q } = o;

  //선언 없는 할당
  const p, q;
  ({ p, q } = { p: 30, q: true });

  //배열 구조 분해
  const arr = [1, 2, 3, 4, 5];
  const [one, two, three, four, five] = arr;
  // 아래와 같다.
  /* 
  const arr = [1,2,3,4,5];
  const one = arr[0]; 
  const two = arr[1]; 
  const three = arr[2]; 
  const four = arr[3]; 
  const five = arr[4]; 
  */
  ```

  위와 같이 객체와 배열에 접근하여 할당이 가능하다. `함수도 그렇다.`<br>
  아래 예제에서 f()는 출력으로 배열[1, 2]을 반환하는데, 하나의 구조 분해만으로 값을 분석할 수 있다.

  ```javascript
  function f() {
    return [1, 2];
  }

  const a, b;
  [a, b] = f();
  console.log(a); // 1
  console.log(b); // 2
  ```

  변수에 배열의 나머지 할당할 때, 나머지 구문을 이용해 분해하고 `남은 부분`을 하나의 변수에 할당할 수 있다.

  ```javascript
  const [a, ...b] = [1, 2, 3];
  console.log(a); // 1
  console.log(b); // [2, 3]
  ```

  그럼 아래의 코드에서 a,c,e속성과 a,b,e을 구조 분해 할당 문법으로 변수에 할당해보자.

  ```javascript
  const obj = {
    a: "hello",
    b: {
      c: "hi",
      d: { e: "wow" },
    },
  };

  //a,c,e
  const {
    a,
    b: {
      c,
      d: { e },
    },
  } = obj;

  //a,b,e
  const { a, b } = obj;
  const {
    d: { e },
  } = b;
  ```

  `리액트`에서도 객체 구조 분해 할당을 예로 들 수 있다.

  ```javascript
  //...
  this.state = {
    like: 0,
    follow: 0,
  };
  //...

  const { like, follow } = this.state;
  ```

  `props`를 받아오거나 `클래스형 컴포넌트`에서의 `state` 값을 쓸 때 구조 분해 할당을 이용해주면 반복되는 당연한 단어들을 줄여줘서 좀 더 편리하다<br>
  마지막으로 로그인 로직의 배열구조 분해할당을 예로 들겠다.

  ```javascript
  //...
  const [shouldBeBearer, token] = req.headers.authorization.split(" ");
  //...
  ```

  로그인 시 발급되는 token을 xxxx xXxxXXXxXxxxxX-XxX...식으로 string형태로 넘어오는데, 띄어쓰기 단위로 split 뒤에 Bearer(xxxx)과 token을 분리하면 앞부분은 필요가 없으니까 token값만 가져다 쓸 수 있다.

  뭔가... 어제 마지막으로 필수적인 내용들을 정리했었는데, 구조분해 할당까지도 마스터한다면 두려울 것이 없어보인다. ~~미래의 나 자신아 이불차라~~<br><br>

- **이벤트 버블링 / 이벤트 캡처링**

  - 실습과 같이 `td태그`에 `addEventListener`를 적용하였는데 실제로 `html`은 `td`의 부모 `tr->table~body태그`까지 모두 클릭이벤트를 받고 있다. 해당 현상을 <u>이벤트 버블링</u>이라고 한다.<br>
    위와 같이 중첩된 요소에서 이벤트가 발생할 때, `HTML DOM API`의 이벤트 전파방식은 `두 가지`가 존재하는데 바로 <u>버블링</u>과 <u>캡처링</u>이다.
    - 버블링: 이벤트가 발생한 요소`(event.target)`부터 window까지 이벤트를 전파한다.
    - 캡처링: window로부터 이벤트가 발생한 요소까지 이벤트를 전파한다.
    - 차이점: 전파 방향의 차이.
  - `WC3`에서 명시한 이벤트 플로우다.
    <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116874900-06ba4380-ac55-11eb-9388-4bde5fe32f79.png height = 420px width = 390px></p>
  - `그럼 어떻게 컨트롤하는가?`

    - `캡처링`과 `버블링`은 이벤트를 등록할 때, 정의할 수 있다.<br>
      이벤트 리스너인 `addEventListener` 메소드를 보겠다.<br><br>
      **<code>target.addEventListener(type, listener[, useCapture]);</code>**<br><br>
    - 세번째 인자인 `useCapture` 는 용어 그대로 `캡처링 여부`를 뜻한다.<br>
      `default` 값이 `false` 이기 때문에, 단순히 사용했다면 버블링을 통해 이벤트를 전파했을 것이다.<br>
      `true` 로 설정해주면 캡처링을 통해 이벤트를 전파한다.<br><br>
      **<code>target.addEventListener("click", function(){}, true);</code>**<br><br>
    - <u>이벤트 전파를 원하지 않는다면</u> 단순히 `e.stopPropagation() `메소드를 사용하면 된다.<br>
      ```javascript
      target.addEventListener("click", function (e) {
        e.stopPropagation();
      });
      ```
    - 실습에서는 `event.target`이 최하위 요소인 td를 가리키는데, 만약 table을 가리키고 싶으면 `event.currentTarget`(실제 이벤트가 발생한 요소)을 사용하면 된다. 실무에선 currentTarget만 쓰는 사람들도 많다고 하는데, 굳이 제한을 두기 보다는 버블링, 캡처링 현상을 잘 이해하여 적용하면 된다.<br>
      참고: 이벤트캡처링을 써서 모달 또는 팝업외 영역 선택 시 창이 닫히는 기능을 구현할 수 있다고 한다. 나중에 해봐야겠다.

    - ```javascript
      let rowIndex = 0;
      let cellIndex = 0;
      rows.forEach((rpw, ri) => {
        rows.forEach((cell, ci) => {
          if (cell === target) {
            rowIndex = ri;
            cellIndex = ci;
          }
        });
      });
      ```
      실습 중 행과 열의 위치를 저장해주는 반복문의 코드를 아래와 같이 바꿀 수 있다.
      ```javascript
      const rowIndex = target.parentNode.rowIndex; //td의 부모태그인 tr을 가져오기 위해선 parentNode를 사용하면 된다.
      const cellIndex = target.cellIndex;
      rows.forEach((rpw, ri) => {
        rows.forEach((cell, ci) => {
          if (cell === target) {
            rowIndex = ri;
            cellIndex = ci;
          }
        });
      });
      ```
      바로 위 부모태그를 선택하고 싶으면 `.parentNode`를 쓰면 된다. 반대로는 `children`이 있다. `children`이 여러 개가 있으면 `유사배열 객체`로 반환한다.
      - **유사배열 객체**
        `태그.children`은 배열처럼 생긴 객체다. `{0:td, 1:td, 2:td, length:3}`과 같은 모양을 가진 객체다. 배열로 착각하기 쉬운데, indexOf 등 배열메서드를 쓰려면 `Array.from`메서드로 유사 배열 객체를 진짜 배열로 바꿀 수 있다.
    - ```javascript
      let draw = true;
      rows.forEach((row) => {
        row.forEach((cell) => {
          if (!cell.textContent) {
            //한 칸이라도 비어있으면
            draw = false; // 무승부가 아니다.
          }
        });
      });
      ```
      위의 2중 반복문에 한칸이라도 비어있을 시에 무승부가 아님을 찾아내는 부분이 있는데, 만약 게임이 다 끝났는데도 for문을 도는 경우가 있으면 그건 최악의 코드가 될 수도 있다.<br>
      `forEach`는 `break, return`으로는 멈출 수 없지만 배열에서 제공하는 `every함수`를 쓰면 된다. 더불어 `some`도 살펴볼거다.<br>
      `array.flat()`으로 2차원 배열을 1차원으로 펴준 뒤에 `every`를 쓰면 된다.
      ```javascript
      rows.flat().every((td) => td.textContent); //td.textContent가 모두 존재해야 true. 아니면 false
      rows.flat().every((td) => !td.textContent); //모두가 존재하지 않으면 true. 아니면 false
      rows.flat().some((td) => td.textContent); //하나라도 존재하면 true. 아니면 false
      rows.flat().some((td) => !td.textContent); //하나라도 없으면 true. 아니면 false
      ```
      즉 실습의 무승부 판단하는 2차원 배열을
      ```javascript
      const draw = rows.flat().every((td) => td.textContent);
      ```
      로 바꿀 수 있다.
      - flat은 3차원 배열을 2차원으로, 2차원 배열을 1차원으로, 1차원은 아무리 해도 1차원으로 반환한다.

<br>

---

<br>

## **반응속도 테스트**

### ` 2021.05.02 response-check.html`<br><br>

### **플로우**

- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116807565-d39d8480-ab6e-11eb-8c35-577dbf3b6aab.png height = 600px width = 700px></p>

### **배운 개념**

- HTML에서 요소에 대한 정보를 저장할 때가 있으니 최대한 이것을 활용하자.
  - 클래스명을 사용하기: 태그에 해당 클래스가 들어있는지는 T`AG.classList.contains('클래스'); = T/F`로 찾을 수 있다.
    - `TAG.classList.add('클래스')`; -->추가 <br>
      ` TAG.classList.replace('클래스')`; -->수정 <br>
      `TAG.classList.remove('클래스')`; -->제거 <br>
- Date객체.

  - `new Date(2021, 06, 02, 18, 30, 5)`; --> `javascript`는 `'월'`만 0 index부터 시작한다.<br>
    결과: `Sun May 02 2021 18:30:05 GMT+0900(대한민국 표준시)` <br>

- ### **`let의 스코프`**

  ```javascript
  $screen.addEventListener("click", (event) => {
    if (event.target.classList.contains("waiting")) {
      $screen.classList.remove("waiting");
      $screen.classList.add("ready");
      $screen.textContent = "초록색이 되면 클릭하세요";
      setTimeout(() => {
        let startTime = new Date(); // 선언
        $screen.classList.remove("ready");
        $screen.classList.add("now");
        $screen.textContent = "클릭하세요!";
      }, Math.floor(Math.random() * 1000) + 2000);
    } else if (event.target.classList.contains("ready")) {
    } else if (event.target.classList.contains("now")) {
      let endTime = new Date(); // 선언
      result = endTime - startTime;
      $result.textContent = `${result}ms`;
      $screen.classList.replace("now", "waiting");
      $screen.textContent = "클릭해서 시작하세요.";
    }
  });
  ```

  1. let은 블록 스코프다. 위와 같이 조건분기 또는 특정 메서드 내에 선언을 하게 되면 그 블록 내에서만 유효하다.<br>
     그럼 모든걸 감싸고 있는 `addEventListener`함수 내에 선언을 해보자.

  ```javascript
  $screen.addEventListener("click", (event) => {
    let startTime; // 선언
    let endTime; // 선언
    if (event.target.classList.contains("waiting")) {
      $screen.classList.remove("waiting");
      $screen.classList.add("ready");
      $screen.textContent = "초록색이 되면 클릭하세요";
      setTimeout(() => {
        startTime = new Date(); //호출
        $screen.classList.remove("ready");
        $screen.classList.add("now");
        $screen.textContent = "클릭하세요!";
      }, Math.floor(Math.random() * 1000) + 2000);
    } else if (event.target.classList.contains("ready")) {
    } else if (event.target.classList.contains("now")) {
      endTime = new Date(); //호출
      result = endTime - startTime;
      $result.textContent = `${result}ms`;
      $screen.classList.replace("now", "waiting");
      $screen.textContent = "클릭해서 시작하세요.";
    }
  });
  ```

  2. 함수 내에 정의를 하면 `addEventListener`가 종료될 떄 변수가 지워지므로 첫번째 클릭 후에 변수 startTime값이 지워지고 두번째 클릭을 하게 되면 startTime값이 없는 상황이므로 result값이 원하는 값으로 나오지 않는다.<br> 아래와 같이 바깥에 전역선언을 해보자.

  ```javascript
  let startTime; // 선언
  let endTime; // 선언
  $screen.addEventListener("click", (event) => {
    if (event.target.classList.contains("waiting")) {
      $screen.classList.remove("waiting");
      $screen.classList.add("ready");
      $screen.textContent = "초록색이 되면 클릭하세요";
      setTimeout(() => {
        startTime = new Date(); //호출
        $screen.classList.remove("ready");
        $screen.classList.add("now");
        $screen.textContent = "클릭하세요!";
      }, Math.floor(Math.random() * 1000) + 2000);
    } else if (event.target.classList.contains("ready")) {
    } else if (event.target.classList.contains("now")) {
      endTime = new Date(); //호출
      result = endTime - startTime;
      $result.textContent = `${result}ms`;
      $screen.classList.replace("now", "waiting");
      $screen.textContent = "클릭해서 시작하세요.";
    }
  });
  ```

  3. 이러면 원하는 결과값이 나오는 것을 볼 수 있다.

- ## **`const`**

```javascript
const a = "123";
a = "456";

//result
//Uncaught TypeError: Assignment to constant variable...
```

```javascript
const array = [1, 2, 3];
array.push(4);
console.log(`${array}`);

// result
// [1,2,3,4]

array = [1, 3, 5];

//result
//Uncaught TypeError: Assignment to constant variable...
```

`const`를 사용하면 해당 변수를 대입할 수가 없다.<br>
그러므로 `const 변수명 = 초기값;`으로 했을 때, 아래에 `변수명 = 대입값;`이라는 개념 자체가 성립할 수가 없다.<br><br>

- ## **`아니 그래서 스코프가 뭐야??`**

  ES6(ES2015)이전에는 변수를 선언할 수 있는 키워드가 var뿐이었다. <br>
  하지만 ES6에서는 `let`,` const` 키워드가 추가되어 이를 이용해서도 변수를 선언할 수 있게 됐다.<br><br>

  **var, let, const의 차이**

  1. `var`는 <u>함수 레벨 스코프</u>이고 `let`,`const`는 <u>블록 레벨 스코프</u>다.<br>
  2. `var`로 선언한 변수는 선언 전에 사용해도 에러가 나지 않지만 `let`, `const`는 에러 발생.<br>
  3. `var`는 이미 선언되어있는 이름과 같은 이름으로 변수를 또 선언해도 에러가 나지 않지만 `let`, `const`는 이미 존재하는 변수와 같은 이름의 변수를 또 선언하면 에러 발생.<br>
  4. `var`, `let`은 변수 선언시 초기 값을 주지 않아도 되지만 `const`는 반드시 초기값 할당 필요.<br>
  5. `var`, `let`은 값을 다시 할당할 수 있지만 `const`는 한번 할당한 값은 변경할 수 없다.(단, 객체 내부의 프로퍼티가 변경되는 것까지 막지는 못한다).<br><br>

  2번의 경우를 보면 ‘그럼 `var`는 호이스팅이 되고 `let`, `const`는 호이스팅이 안되기 때문에 그러는건가?’ 라고 생각할 수 있는데, 그렇지 않다.
  <br>자바스크립트에서 사용하는 `var, let, const, function, class` 등의 선언 키워드들은 모두 호이스팅이 된다.<br>
  `var`의 경우 호이스팅되면서 초기 값이 없으면 자동으로 `undefined`를 초기값으로 하여 메모리를 할당한다. 그래서 `var`의 경우 선언 전에 해당 변수를 사용하려고 해도 메모리에 해당 변수가 존재하기 때문에 에러가 발생하지 않는다.<br><br>

  ```javascript
  console.log(num); // undefined
  var num;
  ```

  그런데 `let, const`의 경우 호이스팅이 되면서 초기 값이 없다면 `var`처럼 자동으로 초기값을 할당하지 않는다. (`const`의 경우 선언시 초기값을 할당하지 않으면 Syntax에러 발생). 그래서 값이 할당되기 전까지 메모리를 할당하지 않기 때문에 선언 전에 (정확히는 값이 할당되기 전에) 사용하려고 하면 메모리에 해당 변수가 존재하지 않아 에러를 발생시킨다. <br>
  이처럼 <u>변수가 선언되고 해당 변수에 값이 할당되기 전까지</u>를 `TDZ(Temporal Dead Zone)`라고 한다.

  ```javascript
  console.log(num); // Uncaught ReferenceError: num is not defined
  let num = 1;
  console.log(num); // 1

  // 호이스팅 되었을 때
  let num;
  console.log(num); // 값이 할당되기 전이므로 아직 num은 메모리에 존재하지 않음. TDZ.
  num = 1;
  console.log(num); // 1
  ```

  5번의 경우 `const`로 선언한 변수에 객체를 할당했을 때 `const` 변수가 가리키고 있는 <u>객체 자체를 변경하는 것은 불가능</u>하지만 <u>해당 객체 내의 프로퍼티가 변경되는 것까지 막는다는 것은 아니다.</u> 아래의 코드를 보자.

  ```javascript
  const obj = { first: 1 };

  // 에러 발생
  obj = 1; // Uncaught TypeError: Assignment to constant variable.

  // 에러 발생하지 않음.
  obj.first = 2;
  obj.second = 2;
  delete obj.first;
  ```

  위와 같이 <u>`const` 변수가 가리키는 값 자체</u>를 변경하려고 하면 에러가 발생하지만 객체 내의 프로퍼티의 <u>추가, 변경, 삭제</u>를 하는 것은 문제가 없다.<br>
  만약 객체의 프로퍼티도 변경되지 않게 객체를 선언하고 싶으면 `Object.freeze()` 메소드를 사용할 수 있다(Object.freeze()는 얕은 동결이기 때문에 객체 내의 또다른 객체의 프로퍼티 변경까지 동결하지는 못한다).<br>

  결론: `var`의 경우 버그 발생과 메모리 누수의 위험 등이 있기 때문에 `var`말고 `let`, `const`를 사용하는 것이 좋다.<br><br>

- ### **reduce**

  ```javascript
  [1, 2, 3, 4].reduce((a, c) => a + c, 0);

  // a는 누적값, c는 현재값, a+c는 다음 누적값, 0은 초기값.
  // a:0 c:1
  // a:1 c:2
  // a:3 c:3
  // a:6 c:4
  // 10
  ```

  - 위와 같이 `배열명.reduce((누적값인자, 현재값인자, 다음 인덱스) => (리턴하는 다음 누적값), 초기값);` 형식으로 작성할 수 있으며, 뜻 그대로 여러개의 배열요소를 하나의 값으로 **축소**해주는 역할을 한다.
    - 정석은 아래와 같다.
      ```javascript
      배열. reduce((누적값,현재값)=>{
        return 새로운 누적값;
      }, 초기값);
      ```
  - 그래서 평균을 구하고 싶으면 `[1,2,3,4].reduce((a,c)=>(a+c),0) / [1,2,3,4].length` (누적값/배열길이)로 표현이 가능하다.
  - 초기값이 없으면 배열 0 index의 1이 초깃값이 된다. 초깃값은 첫 번째 누적값으로 들어가기 때문에 이때는 두 번째 요소부터 `reduce`를 적용하게 된다. 따라서 누적값(a) 1, 현재값(c) 2인 상태로 함수가 시작된다. 반환값인 3은 다음 번의 누적값이 된다.
  - 또한 이렇게도 가능하다

    ```javascript
    ['철수','영희','민초','하와이안'].reduce((a,c,i)=>{a[i]=c; return a},{});

    result
    {0:'철수', 1:'영희', 2: '민초', 3:'하와이안'}

    a:{}, c:'철수', i:0
    a:{0:'철수'}, c:'영희', i:1
    a:{0:'철수', 1:'영희'}, c:'민초', i:2
    a:{0:'철수', 1:'영희', 2:'민초'}, c:'하와이안', i:3
    a:{0:'철수', 1:'영희', 2:'민초', 3:'하와이안'}
    ```

    - 위와 같이 `reduce`를 써서 객체 리터럴 생성이 가능한 것을 볼 수 있다. 중요한건 `초기값`을 무엇으로 했는지 잘 봐야한다. <u>map과 reduce는 가장 중요한 개념이기에 반드시 숙지해야 한다.</u> + `실행컨텍스트`와 `promise`, `이벤트루프`만 마스터하자.

- ## `[일지]` 프론트엔드 개발자가 되기로 한 이유와 고찰.
  - 자바스크립트의 특징: 범용성과 확장성.
  - 생명주기
    - 앞으로 10년 뒤에 세상은 아마 네이티브로 남아있을 수 밖에 없는 몇 안되는 앱을 제외하고는 모두 웹 기반 앱으로 이주하거나 새로운 웹 앱이 생겨날 것 같다. 웹 기반 앱의 가장 큰 단점은 성능인데, 브라우저 및 유저환경의 성능이 계속 향상되면 일반 네이티브 앱 만큼 성능을 낼 수 있도록 빠르게 동작할 것이라 예상한다. 그러면 굳이 네이티브로 만들 필요 없이 웹 기반으로 개발하는 것이 당연하게 될 것 같다.
  - 트렌드의 변화
    - 물론 다른 개발스택도 트렌드는 존재한다. 하지만 강력한 범용성으로 인하여 비즈니스에서도 이 웹개발에 대한 흥미도가 올라가고, 그에 따라 인재풀이 급격하게 증가하여 웹의 수요와 공급이 증가하고 있는 상황이다. 근데 이 스택 마저도 언제까지 이어질진 모르겠으나 T-Leaning의 원칙에 입각하여 급증하고 있는 시장 속에 방대한 정보를 활용해 트랜드를 파악하면서 하나만 파고 지적 욕구를 채워나가기 위함이 이유 중 하나다.
      <br>

---

<br>
    <br>

## **가위바위보**

### `2021.04.30 rsp.html`<br><br>

### **플로우**

- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116676173-9f488d80-a9e1-11eb-846d-c43c20ec34f6.png height = 300px width = 180px></p>

### **배운 개념**

- `style.background`

  - <code>$computer.style.background = `url(${IMG_URL}) 0 0`;</code><br> 스타일의 `background`는 `이미지 X좌표 Y좌표값`을 입력할 수 있다.

- ```javascript
  const rspX = {
    scissors: "0",
    rock: "- 230px",
    paper: "-440px",
  };
  ```

  위와 같이 공통된 속성을 지닌 변수의 모임은 객체로 묶어주는 것이 현명하다.

- ```javascript
  $computer.style.background = `url(${IMG_URL}) ${rspX[computerChoice]} 0`;
  $computer.style.backgroundSize = "auto 200px";
  ```

  - `rspX.computerChoice`하면 안된다. 왜냐하면 rspX객체엔 `computerChoice`라는 문자열 속성이 없기 때문에 `rspX[computerChoice]`로 선택해야 한다.

  - `background`를 수정할 때마다 `size도 초기화`되기 때문에 `항상 background와 backgroundSize는 붙어있어야 한다.`

- `setInterval(함수,반복간격(ms));`

  - `setInterval`함수는 `return`값이 존재한다. 그 값은 타이머에 대한 `아이디(숫자)`로 해당 값을 사용하여 타이머를 제거할 수 있다.
  - `setInterval을 취소`할 수 있는 방법으로 `clearInterval`을 제공한다. <br>`let ID= setInterval(함수,ms)`
  - `setTimeout`함수도` clearTimeout`으로 제거할 수 있지만 `setTimeout`함수 내의 인수로 넣은 함수가 실행되기 전에 `clearTimeout`을 호출해야 한다.

- ### **버그**

  ```javascript
  const clickButton = () => {
    //버튼을 눌렀을 때
    clearInterval(intervalId); // 타이머 멈춤.
    //점수 계산 및 화면 표시
    setTimeout(() => {
      intervalId = setInterval(changeComputerHand, 50); //타이머를 만들 때마다 변수에 저장한 후에 clearInterval해야 한다.
    }, 1000); //1초 뒤에 다시 setInterval.
  };

  $rock.addEventListener("click", clickButton);
  $scissors.addEventListener("click", clickButton);
  $paper.addEventListener("click", clickButton);
  ```

  - 여기서, `setTimeout`설정값 1초 내에 여러 번 클릭하면 그림이 빠르게 돌아간다. 그 후에 버튼을 클릭하면 그림이 멈추지 않는다.<br>
    버튼을 클릭할 떄마다 각각 `setTimeout`타이머가 여러 개 실행되기 떄문이다.
  - 버튼클릭을 하면 `clearInterval`을 수행하므로 문제없다고 생각할 수도 있다.<br>
    하지만 버튼은 `setInterval`을 멈추는 `clearInterval`을 수행할 뿐 `setTimeout`을 멈추는 `clearTimeou`t을 수행하지는 않아서 `버튼을 누른 횟수만큼 setTimeout타이머가 실행되고 각각1초 뒤에 setInterval을 하게 되어 그림이 빠르게 돌아가는거다.`<br><br>

  - ### **가정**

    1. `clickButton`을 1초 내에 10번을 눌렀다.<br>
    2. `interval`의 ID인덱스로 따지면 10개가 생기지만 덮어씌우는 방식이기 때문에 마지막 인덱스를 `intervalID변수`에 넣고 있다.<br>
    3. 1초 뒤에 버튼을 클릭하면 마지막 인덱스만 클리어된다.<br><br>

  - ### **해결 방법**

    1. `clearInterval함수 한번 더 사용.`(추천)
       `clickButton`이 실행되는 최초에 한번 클리어하고<br>
       1초 뒤 실행되는 `setTimeout`에 한번 더 클리어를 하면<br>
       1초루프동안마다 클리어가 반복되므로 해결됨.

    ```javascript
    const clickButton = () => {
      clearInterval(intervalId);

      setTimeout(() => {
        clearInterval(intervalId); //추가.
        intervalId = setInterval(changeComputerHand, 50);
      }, 1000);
    };
    $rock.addEventListener("click", clickButton);
    $scissors.addEventListener("click", clickButton);
    $paper.addEventListener("click", clickButton);
    ```

    2. `removeEventListener메서드 사용(비추천: 실수하기 쉬움)`<br>
       이유: <code>func() !== func()</code>라는<u>(객체 참조관계)</u> 개념을 사용하여 `addEventListener`와 `removeEventListener`내의 해결 법: 함수가 같아야 제거가 되는데 같지 않기 때문에 제거가 안되는 점을 놓치기 때문.<br>
       <code>func() === func()</code>값이 `false`를 반환하기 때문에 객체(함수)를 번수에 넣으면 비교할 수 있다.<br>
       참고) 참조관계를 유지하고 싶으면 변수에 넣으면 된다!

    ```javascript
    const clickButton = () => {
      $rock.removeEventListener("click", clickButton); //추가.
      $scissors.removeEventListener("click", clickButton); //추가.
      $paper.removeEventListener("click", clickButton); //추가.
      //clickButton이 실행되는 최초에 한번 제거

      setTimeout(() => {
        $rock.addEventListener("click", clickButton); //추가.
        $scissors.addEventListener("click", clickButton); //추가.
        $paper.addEventListener("click", clickButton); //추가.
        //1초 뒤 실행되는 setTimeout마다 실행
        intervalId = setInterval(changeComputerHand, 50);
      }, 1000);
    };
    $rock.addEventListener("click", clickButton);
    $scissors.addEventListener("click", clickButton);
    $paper.addEventListener("click", clickButton);
    ```

    3. `플레그 변수 사용`(추천)

    ```javascript
    let clickable = true; //추가.
    const clickButton = () => {
      if (clickable) {
        clearInterval(intervalId);
        clickable = false; //추가.
        setTimeout(() => {
          clickable = true; //추가.
          intervalId = setInterval(changeComputerHand, 50);
        }, 1000);
      }
    };
    ```

      <br>

- **<code>diff === '고양이' || diff === '사자' || diff === '강아지' || diff === '거북이'</code><br>**
  위와 같은 경우는 아래와 같이 바꿀 수 있다.<br>
  **<code>['고양이','사자','강아지','거북이']. includes(diff)</code>**<br>
  또는<br>
  **<code>['고양이','사자','강아지','거북이']. indexOf(diff) > -1</code>**<br>
  <br>

---

## **로또 추첨기(feat.비동기)**

### `lotto.html / lotto-self.html 2021.04.30`<br><br>

### **플로우**

- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116583260-213ca600-a951-11eb-9e37-6febac5a2ac8.png height = 400px width = 140px></p>

### **배운 개념**

- **_비동기란_**

  - 코딩한 순서와 다르게 동작하는 코드.(ex.EventListener(), setTimeout()...)
  - 추가적인 내용은 아래 `var과 let의 차이`에 설명이 있다.<br>

- `map, slice`

  - 둘 다 Array하위 메소드이며, 원본은 변하지 않는다.
  - 하지만 매칭해보면 원본이랑은 다르다.(복사 개념이기 때문)
  - `slice`는(자르기 시작할 인덱스, 끝 인덱스) 다만, 끝 인덱스는 자르기 대상에 포함하지 않는다.
    - 인덱스에 -값이 들어가면 뒤에서부터 읽어들인다.
    - splice는 원본이 변한다.(선택하여 꺼낸다.)<br>

- ### **map**

  - `array.map(callbackFunction(currentValue, index, array),thisArg)`

    - **_`map()`함수는 모든 배열의 값이 `Function`을 실행하는 메소드다!_**
    - `callbackFunction`을 실행한 결과를 가지고 새로운 배열을 만들 때 사용한다.
    - `callbackFunction, thisArg` 총 두개의 매개변수가 있고, `callbackFunction`은 `currentValue,index,array` 3개의 매개변수를 갖는다.

      - `currentValue`: 배열 내 현재 값.
      - `index`: 배열 내 현재 값의 인덱스.
      - `array`: 현재 배열
      - `thisArg`: `callbackFunction` 내에서 `this`로 사용될 값.

    - ```javascript
      const days = ["Mon", "Tue", "Wed", "Thus", "Fri"];
      ```
      위와 같은 변수가 있다고 하자.<br>
      여기서 days.map(...)을 적용한다는 것은 <u>days에 있는 모든 요소에 Function을 실행하고 Function에서 나오는 값을 저장해서 새로운 배열로 만든다</u>는 것이다. (그러므로 원본은 변하지 않는다.)

    1.  간단하게 square root (제곱근)을 구해보자

        ```javascript
        const numbers = [4, 9, 16, 25, 36];
        const result = numbers.map(Math.sqrt);
        console.log(result);

        result[(2, 3, 4, 5, 6)];
        ```

    2.  이번에는 기존 배열에 값의 x2를 한 배열을 생성해 보자. <br>
        다음 세 개는 결과가 같다.

            ```javascript
            const numbers = [ 1,2,3,4,5,6,7,8,9];
            const newNumbers = numbers.map(number =>number *2);
            console.log(newNumbers);

            result
            [2, 4, 6, 8, 10, 12, 14, 16, 18]


            const numbers = [ 1,2,3,4,5,6,7,8,9];
            const newNumbers = numbers.map(function(number){
              return number*2;  // 이렇게 해도 결과는 위와 같다.
              });
            console.log(newNumbers);

            result
            [2, 4, 6, 8, 10, 12, 14, 16, 18]

            const numbers = [ 1,2,3,4,5,6,7,8,9];
            function multiplyTwo(number){
              return number *2;
              }
              const newNumbers = numbers.map(multiplyTwo);
              console.log(newNumbers); // 이렇게 해도 결과가 같다.

            result
            [2, 4, 6, 8, 10, 12, 14, 16, 18]
            ```
            위의 결과들은 다 같다. <br>
            numbers에 곱하기 2한 값이 newNumbers에 똑같이 나타난다.<br>
            map 함수는 기존의 배열을 callbackFunction에 의해 새 배열을 만드는 함수이다.
            그러니 기존의 배열이 변하지는 않는다.<br>

    3.  `map` 함수는 filter함수와 같이 <u>Object(객체)타입</u>도 컨트롤 할 수도 있다.

        ```javascript
        const students = [
          { id: 1, name: "james" },
          { id: 2, name: "tim" },
          { id: 3, name: "john" },
          { id: 4, name: "brian" },
        ];
        ```

        이러한 Object(객체)타입의 변수가 있다.<br>
        여기서 이름만 추출하고 싶으면 <u>Array map</u>을 이용해서 쉽게 추출할 수 있다.

        ```javascript
        const students = [
          { id: 1, name: "james" },
          { id: 2, name: "tim" },
          { id: 3, name: "john" },
          { id: 4, name: "brian" },
        ];
        const names = students.map((student) => student.name);
        console.log(names);

        result[("james", "tim", "john", "brian")];
        ```

        result를 보면 이름만 추출된 것을 볼 수 있다.<br>
        여기서 student.name을 student.id로 바꾸면 id 값만 추출된다.

        ```javascript
        const students = [
          { id: 1, name: "james" },
          { id: 2, name: "tim" },
          { id: 3, name: "john" },
          { id: 4, name: "brian" },
        ];
        var names = students.map((student) => student.id);
        console.log(names);

        result[(1, 2, 3, 4)];
        ```

        결과를 보면 id 값만 추출된 것을 볼 수 있다.

    4.  또다른 Object(객체)를 보자.

        ```javascript
        const testJson = [
          {name : "이건", salary : 50000000},
          {name : "홍길동", salary : 1000000},
          {name : "임신구", salary : 3000000},
          {name : "이승룡", salary : 2000000}
          ];
          const newJson = testJson.map(function(element, index){
            console.log(element);
            const returnObj = {};
            returnObj[element.name] = element.salary;
            return returnObj;
            });
            console.log("newObj");
            console.log(newJson);


            result
            1. console.log(element);
            { name: '이건', salary: 50000000 }
            { name: '홍길동', salary: 1000000 }
            { name: '임신구', salary: 3000000 }
            { name: '이승룡', salary: 2000000 }
            2. console.log(newJson);
            [{ '이건': 50000000 },{ '홍길동': 1000000 },{ '임신구': 3000000 },{ '이승룡': 2000000 }]
        ```

    5.  이번에는 Array를 reverse. <br>
        배열 값에 곱하기 2한 값들을 reverse한다.

            ```javascript
            const numbers = [1,2,3,4,5,6];
            const numbersReverse  = numbers.map(number => number *2).reverse();
            console.log(numbersReverse);

            result
            [ 12, 10, 8, 6, 4, 2 ]
            ```

    6.  `Array`안에 `Array`가 있는 경우

        ```javascript
        const numbers = [
          [1, 2, 3],
          [4, 5, 6],
          [7, 8, 9],
        ]; //array안에 array가 있는 경우
        const newNumbers = numbers.map((array) =>
          array.map((number) => number * 2)
        );
        console.log(newNumbers);

        result[([2, 4, 6], [8, 10, 12], [14, 16, 18])];
        ```

- 피셔-예이츠 셔플 알고리즘
  ```javascript
  while (candidate.length > 0) {
    //무작위 인덱스 뽑기.
    const random = Math.floor(Math.random() * candidate.length);
    //무작위 인덱스로 뽑은 값은 spliceArray배열에 들어있다.
    const spliceArray = candidate.splice(random, 1);
    //배열에 들어있는 값을 꺼낸다.
    const value = spliceArray[0];
    //shuffle배열에 삽입.
    shuffled.push(value);
  }
  ```
  숫자를 무작위로 섞는 방법이다. 먼저 무작위 인덱스를 하나 선택한 후, 그에 해당하는 요소를 새로운 배열로 옮긴다. 이를 반복하다 보면 새 배열에 무작위로 섞인 숫자들이 들어간다.
- `sort()`<br>

  - sort의 (a,b)가 있으며 원본을 바꾼다.<br>
    <code>array.sort(function(a,b){a-b};); --> array.sort((a,b)=>{a-b};)</code><br>
    로 하면 결과값이 오름차순<code>[1,2,3,4,5,6,7,8,9]</code>가 나온다.<br>
    반대로array.sort((a,b)=>{b-a};)</code>로 하면<br>
    내림차순<code>[9,8,7,6,5,4,3,2,1]</code>이 나온다.
  - 만약 원본을 바꾸는 메서드가 있는 경우 대처법은 <code>array.slice().sort((a,b)=>{a-b})</code><br>
    식으로 **slice()를 중간에 적용**시키면 원본은 그대로 냅두고 복사형식으로 처리할 수 있다.

- **`setTimeout()`**

  ```javascript
  setTimeout(() => {
    //실행문
  }, millisecond);
  ```

  - <u>타이머의 시간은 정확한가?</u>
    - 정확하지 않다. 자바스크립트는 기본적으로 한 번에 한 가지 일만 할 수 있다. 따라서 이미 많은 일을 하고 있다면 설정한 시간이 되어도 `setTimeout`에 지정된 작업이 수행되지 않는다. 기존에 하고 있던 일이 끝나야 `setTimeout`에 지정한 작업이 실행된다.

- ## **`var와 let의 차이`**(feat.클로저)

  ~~내가 면접 때 오지게 절었던 부분 중 하나~~

  - 실습파일(lotto.html)의 for문에서 `let i = 0 을 var i = 0`으로 바꾸면 `undefined 6`이 나온다.즉, 모든 i가 6으로 나온다는 것이다.

  - ### **`변수`**

    - 변수는 `스코프`라는 것을 가진다. `var`는 함수 스코프를 가지고 `let`은 블록 스코프를 가진다.
    - ```javascript
      function b(){
        var a = 1;
      }
      console.log(a);

      result
      Uncaught ReferenceError: a is not defined
      ```

      - 위와 같이 함수b 내부에 `var a`를 정의한 후 함수 바깥에서 변수 a를 호출했더니 에러가 발생했다. 이유인 즉슨 var는 함수 내에서만 동작하는 지역변수이기 때문이다.
      - `이와 같이 함수를 경계로 접근 여부가 달라지는 것`을 **함수 스코프**라고 한다.

    - ```javascript
      if (true) {
        var a = 1;
      }
      console.log(a);

      result;
      1;
      ```

    - 함수 범위에 없고 조건문 내에 있기 때문에 a값이 그대로 출력되는 것을 볼 수 있다.
      즉, 함수만 신경쓴다는 것인데 `let`은 조금 다르다.

    - ```javascript
      if(true){
        let a = 1;
      }
      console.log(a);

      result
      Uncaught ReferenceError: a is not defined
      ```

      - ~~이것봐라?~~ 잘 보면 {}(블록)범위 내에서만 살아숨쉬는 앙증맞은 let이라는 것을 확인할 수 있었다.
      - 그럼 `if, switch, for, while(?얘는 case by case)` 등의 함수가 아닌 `블록 내부`에서만 접근이 허용된다는 것을 유추할 수 있다.

  - ### 이제, 실습파일(`lotto.html`)을 보자

    ```javascript
    for (var i = 0; i < winBalls.length; i++) {
      setTimeout(() => {
        console.log(winBalls[i], i);
        drawBall(winBalls[i], $result);
      }, (i + 1) * 1000);
    }
    ```

    위의 코드 중

    ```javascript
    () => {
      console.log(winBalls[i], i);
      drawBall(winBalls[i], $result);
    };
    ```

    이 부분은 `실제 코드와 다른 시점에 실행`된다. 위 코드가 실행이 될 때는 i값은 이미 6이 되어있다. <br> 즉 <br> `for루프를 모두 돌고 i값이 6이 된다` <br> -> `1초가 된다` <br> -> `winBalls[6]이 나온다` <br> ->` winBalls[6]은 winBalls배열의 범위(5)를 넘었기에 undefined로 나온다` <br> -> `log값이 'undefined 6'으로 출력된다` 라는 일련의 과정으로 이해할 수 있다`.<br> ~~나중에 커서 보고 이해 못하는 나 또는 다른 분들에겐 친절하게 루프로 보여주겠다.~~

    `i가 6이 됨`<br>
    `1초 후 콜백 0실행(i=6)`<br>
    `2초 후 콜백 0실행(i=6)`<br>
    `3초 후 콜백 0실행(i=6)`<br>
    `4초 후 콜백 0실행(i=6)`<br>
    `5초 후 콜백 0실행(i=6)`<br>
    `6초 후 콜백 0실행(i=6)`<br>

    <br>

  - **그렇다면 왜 `let`을 쓸 때는 이러한 문제가 발생하지 았았는가?**<br>
    아래를 보자.

  ```javascript
  for (let i = 0; i < winBalls.length; i++) {
    setTimeout(() => {
      console.log(winBalls[i], i);
      drawBall(winBalls[i], $result);
    }, (i + 1) * 1000);
  }
  ```

  `for`문에서 쓰이는 `let`은 하나의 블록마다 `i`가 고정된다. 이것도 블록 스코프의 특성이라고 보면 된다. <br> 따라서 `setTimeout` 콜백 함수 내부의 `i`도 `setTimeout`을 호출할 떄의 `i`와 같은 값이 들어간다.<br><br>

  ## **"아니 그럼 var쓰던 사람들은 저렇게 코드를 짤 수 없는겁니까?"**

  그래서 나온게(?) `클로저`다. 아래를 보자.

  ```javascript
  for (var i = 0; i < winBalls.length; i++) {
    //함수를 소괄호로 감싼다 = 그 자리에서 바로 호출한다.
    (function (j) {
      //j는 매개변수이고 함수블록 안에 갇혀있다.
      setTimeout(() => {
        console.log(winBalls[j], j); // 함수의 인자j는 for문의 var i를 가리킨다.
        drawBall(winBalls[j], $result);
      }, (i + 1) * 1000);
    })(i);
  }
  ```

  클로저는 `함수와 함수 바깥에 있는 변수와의 관계`다.<br> 함수 바깥에 있는 변수를 함수 안에 있는 변수로 바꿔주고 내부 함수의 변수로써 사용하도록 ~~억지~~ 세팅해준거다 ~~그냥 `let`쓰자...~~ <br><br>

  그래도 ~~난 개발지식이 태아수준이라~~ 현상 자체는 알아야 한다.<br>
  함수랑 함수 자체를 바깥에 있는 변수들을 통틀어 `클로저`라 칭하고 그 `클로저`와 `variable`이 만나면 위의 문제가 발생할 수 있다. 근데 모든 케이스가 그렇지 않고 조건이 껴있다. 바로 `비동기`다. <br>
  즉 **`함수 스코프를 지닌 variable` 과 `비동기함수`가 만나면 `클로저`문제가 발생한다.** 그렇다고 무조건 `let`을 쓰는것은 좋지 않다 왜냐하면 레거시에서 `var`을 쓰는 경우가 있기 때문에 해당 현상에 대해선 설명할 수 있어야 한다. <br>

---

<br>

## **숫자야구**

### `2021.04.29 number-baseball.html`<br><br>

### **플로우**

- 최초 플로우

  <p align = "center"><img src = https://user-images.githubusercontent.com/74204327/116469803-922a8200-a8ad-11eb-80d5-03abfac0124a.png width=500px></p>

- 두 번째 플로우(3진 아웃 추가)

  <p align = "center"><img src =https://user-images.githubusercontent.com/74204327/116470169-02390800-a8ae-11eb-85be-4fb506e2cde9.png width=500px></p>

### **배운 개념**

- event.preventDefault(); //기본 동작 막기(메소드이며 form태그의 기본동작인 깜빡거림을 막을 수 있다.)

- `form태그`의 `event.target`은 배열 식으로 내부 요소들에 접근이 가능하다.
  예를 들면 `event.target[0], event.target[1]`식으로 각각의 요소를 SELECT할 수 있다.

- ```javascript
  for (let n = 0; n < 4; n++) {
    const index = Math.floor(Math.random() * (numbers.length - n));
    answer.push(numbers[index]);
    numbers.splice(index, 1);
  }
  ```

  - 무작위로 숫자를 뽑을 때는 `Math.random`메서드를 사용한다. 단, 뽑은 값은 정수가 아니므로 정수가 필요할 때는 `Math.floor`나 `Math.ceil`같은 메서드를 사용해 정수로 바꿔야 한다.

- \***\*indexOf와 includes\*\***

  - ```javascript
    "2345".indexOf(3) === 1;
    "2345".indexOf(6) === -1;
    ["2", "3", "4", "5"].indexOf("5") === 3;
    ["2", "3", "4", "5"].indexOf(5) === -1; // 요소의 자료형까지 같아야함.
    "2345".includes(3) === true;
    ["2", "3", "4", "5"].includes(5) === false;
    ```

    - 공통점
      - 배열이나 문자열에 원하는 값이 들어있는지 찾는 메서드.
    - indexOf: 원하는 값이 들어있다면 해당 인덱스까지 알려주고, _\*\*들어있지 않다면 -1반환_\*\*
    - includes: 더 직관적으로 true/false를 반환한다.

- ```javascript
  if (new Set(input).size !== 4) {
    return alert("중복되지 않게 입력해주세요");
  }
  ```

  - _new Set(input).size !== 4_
    - **_Set은 중복을 허용하지 않는 특수한 배열이다._**
    - new Set('1231')을 하면 Set 내부에는 1,2,3만 들어간다. 그러므로 위의 코드에 중복이 없다면 4가 나오지만 중복이 있다면 4보다 작은 값이 나올 것이다.
    - Set의 길이를 구할 때는 .length가 아닌 .size로 배열의 길이를 잰다.
  - **_alert함수는 undefined를 return한다._** 즉 return undefined와 같고, undefined는 if문에서는 false로 처리하므로 checkInput()함수에 false로 반환한다.

- ```javascript
  if (answer.join("") === value) {
    $logs.textContent = "홈런!";
    return;
  }
  ```

  - `.join()`
    - 배열을 문자열로 바꾸는 메소드.
    - ('')하면 [3,1,4,5]라는 배열이 '3145'라는 문자열로 바뀜. 안하면 '3,1,4,5'로 불러옴.<기본 값 = (',')>

- ```javascript
  if (tries.length >= 9) {
    const message = document.createTextNode(
      "패배. 정답은 ${answer.join("")}"
      );
      $logs.appendChild(message);
      return;
      }
  ```

  - `tries.length>=`를 한 이유: `tries=[]`에 9개가 차 있다면, 10번째 시도 내에서 성공하면 홈런이고 9개가 이미 차있는 상태에서 10번째 시도했을 때 결국 성공하지 못했다면 패배로 출력하도록 하기 위함이다.
  - `.append()`

    - append메서드는 새로 만들고 싶은 값을 바로 옆에 추가한다.
    - createElement를 통하여 각각 텍스트와 html요소를 지정하여 추가가 가능하다.
    - appendChild()는 createTextNode를 사용하고 변수에 저장 후에 변수를 인자로 넣어야한다. --> append는 appendChild를 보완해서 만든거다.<br>

---

<br>

## **계산기**

### `2021.04.28 calculator.html`<br><br>

### **플로우**

1. 시작 -> 숫자 1을 저장할 변수생성 -> 연산자를 저장할 변수생성 -> 숫자2를 저장할 변수생성 -> 대기
2. 숫자 버튼 클릭 -> 숫자를 변수에 저장한다{operator변수가 비어있는가 ? numOne=숫자 : numTow=숫자} -> 대기
3. 연산자 버튼 클릭 ->numOne값이 존재하는가? 연산자를 변수에 저장한다:alert('연산자를 입력해주세요') -> 대기
4. =버튼 클릭 -> numTwo값이 존재하는가? 숫자1과2를 연산자를 적용하여 계산한다: alert('두번째 수를 입력해주세요')-> 계산 결과를 화면에 출력한다. -> 끝

### **배운 개념**

- 변수를 바꾸면 화면을 바꾸는 것을 까먹지 말자! --> `$result.value += "0";`

- 함수의 (미친듯한)중복을 제거할 때

  - 달라지는 부분만 매개변수로 만들자
  - 고차함수를 써서 줄일 수 있다.(함수 내에 함수 존재)

- `고차함수`<br>
  click(~~~, onClickNumber('n'));의 onClickNumber가 있는 자리는 함수자리이므로 "함수값으로 리턴된 값"이 와야한다!
  추가로 onClickNumber()함수를 불러오면 undefined가 자동으로 도출되기 때문에 return에 함수형으로 정의해야하며, 중복제거시에 유용한 고차함수를 써서 더 줄이자!<br>

  ## 2021.04.30 추가

  - function()는 함수 자체가 아니다. function()은 함수의 리턴값을 가리킨다.
  - function은 함수 자체를 가리킨다.
  - 따라서 return을 따로 지정하지 않으면 undefined가 리턴되기 때문에 function()은 undefined를 가리킨다!

  예시:

  ```javascript
  const fuc = (msg) => {
    return () => {
      console.log(msg);
    };
  };
  ```

  중괄호와 리턴을 생략할 수 있으니 요놈이<br>

  ```javascript
  const fuc = (msg) => () => {
    console.log(msg);
  };
  ```

  이렇게 바뀌는데 이러한 코드를 보면 항상 머리로만 하지 말고 직접 쳐보고 비교해보는 버릇을 기르자. 원리는 이렇다.<br>

  ```javascript
  const fuc =(msg) => "{"
  "return"() =>{
     console.log(msg);
     }
  "}"
  ```

  위와 같이 쿼테이션("")으로 묶은 부분을 제거한거다.<br><br>

- **event**: click시 함수가 호출되면 브라우저가 `onClickNumber()`식으로 함수를 실행하는데, 그때 인자값으로 `event`를 넣어주는데 그때 함수에 `event`가 전달된다.

- \***\*_중첩 반복문 해결하는 법_\*\***

  ```javascript
  function test() {
    let result = "";
    if (a) {
      if (!b) {
        result = "c";
      }
    } else {
      result = "a";
    }
    result += "b";
    return result;
  }
  ```

  <br>

  1. 'if문 다음에 나오는' 공통된 절차를 각 분기점 내부에 넣는다.

  ```javascript
  function test() {
    let result = "";
    if (a) {
      if (!b) {
        result = "c";
      }
      result += "b";
      return result;
    } else {
      result = "a";
      result += "b";
      return result;
    }
  }
  ```

  <br>

  2. 분기점에서 '짧은 절차'부터 실행하게 if문을 작성한다.

  ```javascript
  function test() {
    let result = "";
    if (!a) {
      //기존조건에서 반대 조건으로 변경.
      result = "a";
      result += "b";
      return result;
    } else {
      if (!b) {
        result = "c";
      }
      result += "b";
      return result;
    }
  }
  ```

  <br>

  3. 짧은 절차가 끝나면 `return`이나 `break`로 중단한다.

  ```javascript
  function test() {
    let result = "";
    if (!a) {
      result = "a";
      result += "b";
      return result; //이미 리턴이 있다.
    } else {
      if (!b) {
        result = "c";
      }
      result += "b";
      return result;
    }
  }
  ```

  <br>

  4. `else`를 제거한다.(이때 중첩 하나가 제거된다.)

  ```javascript
  function test() {
    let result = "";
    if (!a) {
      result = "a";
      result += "b";
      return result;
    }
    if (!b) {
      result = "c";
    }
    result += "b";
    return result;
  }
  ```

  <br>

  5. 다음 중첩된 분기점이 나올 때 1~4의 과정을 반복한다.<br><br>

- -, \*, /는 문자열이 저절로 숫자로 바뀌기 때문에 `parseInt`를 안써도 된다.

---

<br>

## **끝말잇기**

### `2021.04.23 word-relay.html / kungkungdda.html` <br><br>

### **배운 개념**

`addEventListener(이벤트종류,'함수')`, `querySelector`, `querySelectorAll`, `parseInt`.

- 느낀 점: 천재가 아닌 이상 순서도를 그려보고 절차적이며 논리적으로 생각하는 습관을 기르자.
  고로 난 천재가 아니니 문제 생기면 순서도 그리자.

---

<br>

## **자바스크립트 기초**

### `2021.04.23 1stJsStudy.html` <br><br>

### **배운 개념**

- 선택자$는 태그를 변수에 넣을 때 사용한다., hide, show.

---
