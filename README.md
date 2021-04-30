<!-- prettier-ignore -->
# **`JavaScript`**

Study for javascript by WaterMincho.<br>
민초의 자바공부.<br><br>

---
<br>

## 자바스크립트 기초 
### `2021.04.23 1stJsStudy.html` <br><br>

### 배운 개념

- 선택자$는 태그를 변수에 넣을 때 사용한다., hide, show.

---
<br>

## 끝말잇기 
### `2021.04.23 word-relay.html / kungkungdda.html` <br><br>

### 배운 개념

`addEventListener(이벤트종류,'함수')`, `querySelector`, `querySelectorAll`, `parseInt`.

- 느낀 점: 천재가 아닌 이상 순서도를 그려보고 절차적이며 논리적으로 생각하는 습관을 기르자.
  고로 난 천재가 아니니 문제 생기면 순서도 그리자.

---

## 계산기 
### `2021.04.28 calculator.html`<br><br> 

### 플로우

1. 시작 -> 숫자 1을 저장할 변수생성 -> 연산자를 저장할 변수생성 -> 숫자2를 저장할 변수생성 -> 대기
2. 숫자 버튼 클릭 -> 숫자를 변수에 저장한다{operator변수가 비어있는가 ? numOne=숫자 : numTow=숫자} -> 대기
3. 연산자 버튼 클릭 ->numOne값이 존재하는가? 연산자를 변수에 저장한다:alert('연산자를 입력해주세요') -> 대기
4. =버튼 클릭 -> numTwo값이 존재하는가? 숫자1과2를 연산자를 적용하여 계산한다: alert('두번째 수를 입력해주세요')-> 계산 결과를 화면에 출력한다. -> 끝

### 배운 개념

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

- ****_중첩 반복문 해결하는 법_****

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

## 숫자야구 
### `2021.04.29 number-baseball.html`<br><br>

### 플로우

- 최초 플로우

  <p align = "center"><img src = https://user-images.githubusercontent.com/74204327/116469803-922a8200-a8ad-11eb-80d5-03abfac0124a.png width=500px></p>

- 두 번째 플로우(3진 아웃 추가)

  <p align = "center"><img src =https://user-images.githubusercontent.com/74204327/116470169-02390800-a8ae-11eb-85be-4fb506e2cde9.png width=500px></p>

### 배운 개념

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


- ****indexOf와 includes****
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
    - indexOf: 원하는 값이 들어있다면 해당 인덱스까지 알려주고, _**들어있지 않다면 -1반환_**
    - includes: 더 직관적으로 true/false를 반환한다.


- ```javascript
  if (new Set(input).size !== 4) {
    return alert("중복되지 않게 입력해주세요");
    }
  ```

  - *new Set(input).size !== 4*
    - **_Set은 중복을 허용하지 않는 특수한 배열이다._**
    - new Set('1231')을 하면 Set 내부에는 1,2,3만 들어간다. 그러므로 위의 코드에 중복이 없다면 4가 나오지만 중복이 있다면 4보다 작은 값이 나올 것이다.
    - Set의 길이를 구할 때는 .length가 아닌 .size로 배열의 길이를 잰다.
  - ***alert함수는 undefined를 return한다.*** 즉 return undefined와 같고, undefined는 if문에서는 false로 처리하므로 checkInput()함수에 false로 반환한다.



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
    - appendChild()는 createTextNode를 사용하고 변수에 저장 후에 변수를 인자로 넣어야한다. --> append는 appendChild를 보완해서 만든거다.

    ---

## 로또 추첨기(feat.비동기)
### `lotto.html / lotto-self.html 2021.04.30`<br><br>

### 플로우

- 
<p align="center"><img src = https://user-images.githubusercontent.com/74204327/116583260-213ca600-a951-11eb-9e37-6febac5a2ac8.png height = 400px width = 140px></p>

### 배운 개념

- ***비동기란***
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
    - ***`map()`함수는 모든 배열의 값이 `Function`을 실행하는 메소드다!***
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

    1. 간단하게 square root (제곱근)을 구해보자

          ```javascript 
          const numbers = [4,9,16,25,36];
          const result = numbers.map(Math.sqrt);
          console.log(result);
        
          result
          [ 2, 3, 4, 5, 6 ]
          ```
    2. 이번에는 기존 배열에 값의 x2를 한 배열을 생성해 보자. <br>
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
          
    3. `map` 함수는 filter함수와 같이 <u>Object(객체)타입</u>도 컨트롤 할 수도 있다.     
        
          ```javascript 
          const students = [
            {id:1, name:"james"},
            {id:2, name:"tim"},
            {id:3, name:"john"},
            {id:4, name:"brian"}
            ];
          ```

          이러한 Object(객체)타입의 변수가 있다.<br>
          여기서 이름만 추출하고 싶으면 <u>Array map</u>을 이용해서 쉽게 추출할 수 있다.
         ```javascript
          const students = [
            {id:1, name:"james"},
            {id:2, name:"tim"},
            {id:3, name:"john"},
            {id:4, name:"brian"}
            ];
          const names = students.map(student =>student.name);
          console.log(names); 

          result
          ['james', 'tim', 'john', 'brian']
          ```

          result를 보면 이름만 추출된 것을 볼 수 있다.<br>
          여기서 student.name을 student.id로 바꾸면 id 값만 추출된다.

          ```javascript
          const students = [
            {id:1, name:"james"},
            {id:2, name:"tim"},
            {id:3, name:"john"},
            {id:4, name:"brian"}
            ];
            var names = students.map(student =>student.id);
            console.log(names); 
            
            result
            [ 1, 2, 3, 4 ]
          ```
          결과를 보면 id 값만 추출된 것을 볼 수 있다.
            
    4. 또다른 Object(객체)를 보자.
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

    5. 이번에는 Array를 reverse. <br>
    배열 값에 곱하기 2한 값들을 reverse한다.

        ```javascript
        const numbers = [1,2,3,4,5,6];
        const numbersReverse  = numbers.map(number => number *2).reverse();
        console.log(numbersReverse);

        result
        [ 12, 10, 8, 6, 4, 2 ]
        ```

    6. `Array`안에 `Array`가 있는 경우
        ```javascript
        const numbers = [[1,2,3],[4,5,6],[7,8,9]]; //array안에 array가 있는 경우
        const newNumbers = numbers.map(array => array.map(number => number *2));
        console.log(newNumbers);
        
        result
        [ [ 2, 4, 6 ], [ 8, 10, 12 ], [ 14, 16, 18 ] ]
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
    setTimeout(()=>{
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
      if(true){
        var a = 1;
      }
      console.log(a);

      result
      1
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
            console.log(winBalls[i],i);
            drawBall(winBalls[i], $result);
            }, (i + 1) * 1000); 
          }
    ```
    위의 코드 중

    ```javascript
        () => {
          console.log(winBalls[i],i);
          drawBall(winBalls[i], $result);
           } 
    ```
    이 부분은 `실제 코드와 다른 시점에 실행`된다. 위 코드가 실행이 될 때는 i값은 이미 6이 되어있다. <br> 즉 <br> `for루프를 모두 돌고 i값이 6이 된다` <br> -> `1초가 된다` <br> -> `winBalls[6]이 나온다` <br> ->` winBalls[6]은 winBalls배열의 범위(5)를 넘었기에 undefined로 나온다` <br> -> `log값이 'undefined 6'으로 출력된다` 라는 일련의 과정으로 이해할 수 있다`.<br> ~~나중에 커서 보고 이해 못하는 나 또는 다른 분들에겐 친절하게  루프로 보여주겠다.~~

    
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
            console.log(winBalls[i],i);
            drawBall(winBalls[i], $result);
            }, (i + 1) * 1000); 
          }
    ```
    `for`문에서 쓰이는 `let`은 하나의 블록마다 `i`가 고정된다. 이것도 블록 스코프의 특성이라고 보면 된다. <br> 따라서 `setTimeout` 콜백 함수 내부의 `i`도 `setTimeout`을 호출할 떄의 `i`와 같은 값이 들어간다.<br><br>

    ## **"아니 그럼 var쓰던 사람들은 저렇게 코드를 짤 수 없는겁니까?"**
    그래서 나온게(?) `클로저`다. 아래를 보자.
    ```javascript
      for (var i = 0; i < winBalls.length; i++) {//함수를 소괄호로 감싼다 = 그 자리에서 바로 호출한다.
          (function(j) { 
          //j는 매개변수이고 함수블록 안에 갇혀있다.
            setTimeout(() => {
            console.log(winBalls[j],j); // 함수의 인자j는 for문의 var i를 가리킨다.
            drawBall(winBalls[j], $result);
            }, (i + 1) * 1000); 
          })(i);
        }
    ```

    클로저는 `함수와 함수 바깥에 있는 변수와의 관계`다.<br> 함수 바깥에 있는 변수를 함수 안에 있는 변수로 바꿔주고 내부 함수의 변수로써 사용하도록 ~~억지~~ 세팅해준거다 ~~그냥 `let`쓰자...~~ <br><br>
    
    그래도 ~~난 개발지식이 태아수준이라~~ 현상 자체는 알아야 한다.<br>
    함수랑 함수 자체를 바깥에 있는 변수들을 통틀어 `클로저`라 칭하고 그 `클로저`와 `variable`이 만나면 위의 문제가 발생할 수 있다. 근데 모든 케이스가 그렇지 않고 조건이 껴있다. 바로 `비동기`다. <br> 
    즉 **`함수 스코프를 지닌 variable` 과 `비동기함수`가 만나면 `클로저`문제가 발생한다.** 그렇다고 무조건 `let`을 쓰는것은 좋지 않다 왜냐하면 레거시에서 `var`을 쓰는 경우가 있기 때문에 해당 현상에 대해선 설명할 수 있어야 한다. <br>