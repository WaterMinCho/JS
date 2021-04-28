<!-- prettier-ignore -->
# **JavaScript**

Study for javascript by WaterMincho.
민초의 자바공부.

---

## 자바스크립트 기초

1stJsStudy.html

### 배운 개념

- 선택자$는 태그를 변수에 넣을 때 사용한다., hide, show.

## 끝말잇기

word-relay.html

### 배운 개념

addEventListener(이벤트종류,'함수'), querySelector, querySelectorAll, parseInt.

- 느낀 점: 천재가 아닌 이상 순서도를 그려보고 절차적이며 논리적으로 생각하는 습관을 기르자.
  고로 난 천재가 아니니 문제 생기면 순서도 그리자.

---

## 계산기

### 플로우

1. 시작 -> 숫자 1을 저장할 변수생성 -> 연산자를 저장할 변수생성 -> 숫자2를 저장할 변수생성 -> 대기
2. 숫자 버튼 클릭 -> 숫자를 변수에 저장한다{operator변수가 비어있는가 ? numOne=숫자 : numTow=숫자} -> 대기
3. 연산자 버튼 클릭 ->numOne값이 존재하는가? 연산자를 변수에 저장한다:alert('연산자를 입력해주세요') -> 대기
4. =버튼 클릭 -> numTwo값이 존재하는가? 숫자1과2를 연산자를 적용하여 계산한다: alert('두번째 수를 입력해주세요')-> 계산 결과를 화면에 출력한다. -> 끝

### 배운 개념

- 변수를 바꾸면 화면을 바꾸는 것을 까먹지 말자! --> $result.value += "0";

- 함수의 (미친듯한)중복을 제거할 때

  - 달라지는 부분만 매개변수로 만들자
  - 고차함수를 써서 줄일 수 있다.(함수 내에 함수 존재)

- 고차함수
  click(~~~, onClickNumber('n'));의 onClickNumber가 있는 자리는 함수자리이므로 "함수값으로 리턴된 값"이 와야한다!
  추가로 onClickNumber()함수를 불러오면 undefined가 자동으로 도출되기 때문에 return에 함수형으로 정의해야하며, 중복제거시에 유용한 고차함수를 써서 더 줄이자!</br>

  예시:

  ```javascript
  const fuc = (msg) => {
    return () => {
      console.log(msg);
    };
  };
  ```

  중괄호와 리턴을 생략할 수 있으니 요놈이</br>

  ```javascript
  const fuc = (msg) => () => {
    console.log(msg);
  };
  ```

  이렇게 바뀌는데 이러한 코드를 보면 항상 머리로만 하지 말고 직접 쳐보고 비교해보는 버릇을 기르자. 원리는 이렇다.</br>

  ```javascript
  const fuc =(msg) => "{"
  "return"() =>{
     console.log(msg);
     }
  "}"
  ```

  위와 같이 쿼테이션("")으로 묶은 부분을 제거한거다.

- event: click시 함수가 호출되면 브라우저가 onClickNumber()식으로 함수를 실행하는데, 그때 인자값으로 event를 넣어주는데 그때 함수에 event가 전달된다.

- _중첩 반복문 해결하는 법_

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

  - 1. 'if문 다음에 나오는' 공통된 절차를 각 분기점 내부에 넣는다.

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

  3. 짧은 절차가 끝나면 return이나 break로 중단한다.

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

  4. else를 제거한다.(이때 중첩 하나가 제거된다.)

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

  5. 다음 중첩된 분기점이 나올 때 1~4의 과정을 반복한다.

- -, \*, /는 문자열이 저절로 숫자로 바뀌기 때문에 parseInt안써도 된다.

---

## 숫자야구

### 플로우

- 최초 플로우

  ![2021-04-29 02 09 12](https://user-images.githubusercontent.com/74204327/116469803-922a8200-a8ad-11eb-80d5-03abfac0124a.png)

- 두 번째 플로우(3진 아웃 추가)

  ![2021-04-29 04 26 23](https://user-images.githubusercontent.com/74204327/116470169-02390800-a8ae-11eb-85be-4fb506e2cde9.png)

### 배운 개념

- event.preventDefault(); //기본 동작 막기(메소드이며 form태그의 기본동작인 깜빡거림을 막을 수 있다.)


- form태그의 event.target은 배열 식으로 내부 요소들에 접근이 가능하다.
  예를 들면 event.target[0], event.target[1]식으로 각각의 요소를 SELECT할 수 있다.


- ```javascript
  for (let n = 0; n < 4; n++) {
    const index = Math.floor(Math.random() * (numbers.length - n));
    answer.push(numbers[index]);
    numbers.splice(index, 1);
    }
    ```
  
  - 무작위로 숫자를 뽑을 때는 Math.random메서드를 사용한다. 단, 뽑은 값은 정수가 아니므로 정수가 필요할 때는 Math.floor나 Math.ceil같은 메서드를 사용해 정수로 바꿔야 한다.


- **indexOf와 includes**
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
    - indexOf: 원하는 값이 들어있다면 해당 인덱스까지 알려주고, _들어있지 않다면 -1반환_
    - includes: 더 직관적으로 true/false를 반환한다.


- ```javascript
  if (new Set(input).size !== 4) {
    return alert("중복되지 않게 입력해주세요");
    }
  ```

  - *new Set(input).size !== 4*
    - _Set은 중복을 허용하지 않는 특수한 배열이다._
    - new Set('1231')을 하면 Set 내부에는 1,2,3만 들어간다. 그러므로 위의 코드에 중복이 없다면 4가 나오지만 중복이 있다면 4보다 작은 값이 나올 것이다.
    - Set의 길이를 구할 때는 .length가 아닌 .size로 배열의 길이를 잰다.
  - *alert함수는 undefined를 return한다.* 즉 return undefined와 같고, undefined는 if문에서는 false로 처리하므로 checkInput()함수에 false로 반환한다.



- ```javascript
  if (answer.join("") === value) {
    $logs.textContent = "홈런!";
    return;
  }
  ```

  - .join()
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

  - tries.length>=를 한 이유: tries=[]에 9개가 차 있다면, 10번째 시도 내에서 성공하면 홈런이고 9개가 이미 차있는 상태에서 10번째 시도했을 때 결국 성공하지 못했다면 패배로 출력하도록 하기 위함이다.
  - .append()
    - append메서드는 새로 만들고 싶은 값을 바로 옆에 추가한다.
    - createElement를 통하여 각각 텍스트와 html요소를 지정하여 추가가 가능하다.
    - appendChild()는 createTextNode를 사용하고 변수에 저장 후에 변수를 인자로 넣어야한다. --> append는 appendChild를 보완해서 만든거다.
