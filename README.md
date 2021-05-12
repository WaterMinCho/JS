<!-- prettier-ignore -->
# **`JavaScript`**

Study for javascript by WaterMincho.<br>
민초의 자바공부.<br><br>

README는 나중에 위키나 개인 블로그로 정리 하기 전 일괄기록이기 때문에 일단 최신 순으로 정리하였다.

---

## **카드 짝맞추기(feat.이벤트루프)**

### `2021.05.12 concentration.html`<br><br>

### **플로우**

- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/117998950-56002280-b37f-11eb-8b41-cc893439d320.png height = 350px width = 430px></p><br>

### **배운 개념**

- concat()

  - concat()메서드는 인자로 주어진 배열이나 값들을 기존 배열에 합쳐서 새 배열을 반환한다.
    - 메서드를 호출한 배열 뒤에 각 인수를 순서대로 붙인다.
    - 인수가 배열이면 그 요소가 순서대로 붙고, 배열이 아니면 인수 자체가 붙는다. 중첩 배열 내부로 재귀하지 않는다.
    - `기존 배열을 변경하지 않는다!`
    - 추가된 새 배열을 반환한다.

- 버그

  - 버그1: 처음 카드 보여줄 때 카드 클릭이 가능하면 안된다.
  - `clickable` 변수로 막을 수 있다.
    ```javascript
    let clickable = false;
    .
    .
    .
    function onCLickCard() {
        if (!clickable) { //clickable이 false면 onClickCard실행 불가.(return으로 아랫줄 실행 막음)
          return;
        }
        .
        .
        .
    }
    function startGame() {
        clickable = false;
        .
        .
        .
        setTimeout(() => {
          document.querySelectorAll(".card").forEach((card) => {
            card.classList.remove("flipped");
          });
          clickable = true; //카드를 감출 때 true값으로 변환.
        }, 5000);
      }
    ```
  - 버그2, 버그3: 이미 짝이 맞춰진 카드를 선택해도 카드가 다시 뒤집힌다. / 한 카드를 두 번 연달아 클릭하면 더 이상 그 카드가 클릭되지 않는다.

  - 버그1에서 수정한 if조건문에 추가한다.

  ```javascript
      function onCLickCard() {
        if (!clickable || completed.includes(this) ||clicked[0]===this) { //이미 완성된 카드거나 연달아 클릭하는 경우
        return;
        }
        .
        .
        .
        }
  ```

  - <u>`버그4`: 서로 다른 네 가지 색의 카드를 연달아 클릭하면 마지막 두 카드가 앞면을 보인 채 남아 있다.(중요. `이벤트루프와 호출스택`을 알아야 수정이 가능하다.</u>
  - **_호출 스택, 이벤트 루프, 백그라운드, 태스크 큐_**
    - **백그라운드**는 타이머를 처리하고 이벤트 리스너를 저장하는 공간이다. `setTimeout`함수가 실행되면 백그라운드에서 시간을 재고 해당 시간이 되면 `setTimeout`의 `콜백 함수`를 `태스크 큐`로 전달한다. <br>
      또한 `addEventListener`로 추가한 이벤트를 저장했다가 이벤트가 발생하면 `콜백 함수`를 `태스크 큐`로 보낸다. `백그라운드`에서 코드를 실행하는 것이 아니라 실행될 `콜백 함수`들이 `태스크 큐`로 들어간다는 것을 명심하자. <br>
    - **태스크 큐**는 실행돼야 할 콜백 함수들이 순서대로 대기하고 있는 공간이다. 즉 `태스크 큐`에 먼저 들어온 함수부터 실행된다는 것이다. 하지만 `태스크 큐`도 함수를 직접 실행하지 않는다. <br>
    - **이벤트 루프**는 `태스크 큐`에서 `호출 스택`으로 함수를 이동시키는 존재다. `호출 스택`이 비어 있으면 `이벤트 루프`는 `태스크 큐`에서 함수를 순서대로 꺼내어 `호출 스택`으로 옮긴다. 호출 스택으로 이동한 함수는 그때 실행된다. 실행이 완료된 함수는 `호출 스택`에서 빠지고 `호출 스택`이 비어있으면 `이벤트 루프`는 `태스크 큐`에 있는 다음 함수를 `호출 스택`으로 옮긴다.
      <br><br> - 변수나 함수의 선언은 `호출 스택`과 `이벤트 루프`에 영향을 주지 않는다! - 선언은 `스코프`의 영역이다. - `호출 스택`과 `이벤트 루프`는 함수 호출과 밀접한 관련이 있다. - 자바스크립트 엔진은 자바스크립트 소스 코드가 처음 실행되는 순간(script태그)도 하나의 함수가 실행된다고 판단하고 크롬은 이를 `anonymous함수`로 표시한다. 참고로 호출 스택을 보고 싶다면 `console.trace`를 사용하여 콘솔창에 출력가능하다.
      <br><br>
  - 실습코드를 보면 호출 스택의 흐름은 anonymous인 -> startGame()인 -> shuffle()인-> shuffle()아웃 -> createCard()인 -> createCard()아웃 -> appendChild()인 -> appendChild()아웃 ... 식으로 흘러가며 addEventListener와 setTimeout()를 만나면 해당 이벤트는 백그라운드에 저장한다.
  - 그럼 `startGame()`이 끝나면 호출 스택에서 대기중인 함수는 사실상 없지만 `백그라운드`에는 `클릭이벤트와 타이머`가 남아있다는 사실! 그러면 대부분의 비동기코드들이 백그라운드에 있다고 생각하면 될 것 같다.

  ```javascript
  document.querySelectorAll(".card").forEach((card, index) => {
    setTimeout(() => {
      card.classList.add("flipped");
    }, 1000 + 200 * index);
  });
  ```

  - 위 코드에서 1초타이머의 콜백함수는 card.classList.add("flipped"); 이 구간인데 이 구간이 바로 `태스크 큐`에 쌓여있고 방금같은 상황에 호출스택이 비어있으니 `이벤트루프`가 `태스크 큐`의 해당 함수를 `호출 스택`으로 전달한다(실행을 하려면 호출 스택으로 쌓여져있어야 하기 때문).

  - ### 그럼 뭐가 문젠데?

    - 4개를 연속 선택했을 때 연속된 카드를 1,2,3,4번 카드라고 가정해보자.<br>

      1. 호출 스택에 클릭 콜백함수를 거쳐 `push()`로 실행되어 clicked === [1번 카드] 상태
      2. 2번 카드의 클릭 콜백 함수가 실행될 때는 clicked배열에 두 카드가 추가
      3. 색이 다르니 `setTimeout`이 실행될거고 0.6초 타이머가 백그라운드에 생성되어 대기상태. 여기서 핵심을 알 수 있다. 태스크 큐에 들어온 순서대로 호출 스택으로 가다보니 0.6초 타이머의 콜백함수보다 3번 카드의 클릭 콜백 함수가 먼저 실행되는 것이다.
      4. 3번의 시점에선 호출 스택으로 `clicked배열`에 [1,2]가 들어가 있고 `태스크 큐`엔 3번4번이 대기중이며, `백그라운드`엔 addEventListener(12), setTimeout 600ms가 대기중이다.
      5. 3번이 호출 스택에 추가되고 1번을 거친다. `clicked`===[1,2,3] / `태스크 큐:` 4번 / `백그라운드:` addEventListener(12), setTimeout 600ms, setTimeout 600ms
      6. `clicked`===[1,2,3,4] / `태스크 큐:` empty / `백그라운드:` addEventListener(12), setTimeout 600ms,setTimeout 600ms, setTimeout 600ms
      7. `clicked`===[1,2,3,4] / `태스크 큐:` setTimeout 600ms,setTimeout 600ms, setTimeout 600ms / `백그라운드:` addEventListener(12)
      8. setTimeout600ms의 콜백함수인

      ```javascript
      clicked[0].classList.remove("flipped");
      clicked[1].classList.remove("flipped");
      clicked = [];
      ```

      를 실행하는데 위의 과정으로 <u>3번과 4번은 remove('flipped')가 적용이 안된 채로 남아있고 clicked 배열을 비우는 것이 문제인걸 확인했다..</u>
      <br><br>
      카드를 뒷면으로 뒤집고 clicked를 []로 초기화하기 전에 3,4번 카드의 클릭 이베느 콜백이 실행되는게 문제기 때문에 실행되더라도 아무 일도 하지 않게 만들면 된다.
      <br>
      그렇다면 **카드가 2장이 될 때 clickable을 true로 만들어서 세 번째 카드부터는 클릭해도 아무것도 하지 않고 끝나게 하자**
      <br>
      카드가 2장이 될 때 `clickable`을 `false`로 만들어서 세 번째 카드부터는 클릭해도 아무것도 하지 않고 끝나게 하자.

      ```javascript
      function onCLickCard() {
        if (!clickable || completed.includes(this) || clicked[0] === this) {
          return;
        }
        this.classList.toggle("flipped");
        clicked.push(this);
        if (clicked.length !== 2) {
          return;
        }
        const firstBackColor =
          clicked[0].querySelector(".card-back").style.backgroundColor;
        const secondBackColor =
          clicked[1].querySelector(".card-back").style.backgroundColor;
        if (firstBackColor === secondBackColor) {
          completed.push(clicked[0]);
          completed.push(clicked[1]);
          clicked = [];

          if (completed.length !== total) {
            return;
          }
          setTimeout(() => {
            alert("축하합니다.");
            resetGame();
          }, 1000);

          return;
        }
        clickable = false; //추가
        setTimeout(() => {
          clicked[0].classList.remove("flipped");
          clicked[1].classList.remove("flipped");
          clicked = [];
          clickable = true; //추가
        }, 600);
      }
      ```

- 느낀 점: 코드를 작성할 때는 스코프를 고려해야 하고 짜여진 코드를 분석할 땐 이벤트루프개념을 고려해야겠다는 생각이 들었다. 그리고 실행컨텍스트를 따로 정리하는 시간을 가져야겠다는 생각도 들었다. (반드시 하자!)
  <br><br>

---

<br>
## **텍스트RPG게임(feat.클래스)**

### `2021.05.08 text-rpg.html / text-rpg-class.html`<br><br>

### **플로우**

- 두 가지 모드가 있다. 모험, 휴식, 종료 중에서 선택하는 일반모드와 모험을 떠나 적을 만나게 될 때 돌입하는 전투 모드다. 전투 모드에서는 적을 공격하거나 체력을 회복. 또는 도망간다.

  - 일반 모드일 때.<p align="center"><img src = https://user-images.githubusercontent.com/74204327/117518428-a91e5200-afda-11eb-894e-e7a75f4791ab.png height = 300px width = 430px></p><br>
  - 전투 모드일 때.<p align="center"><img src = https://user-images.githubusercontent.com/74204327/117518519-fc90a000-afda-11eb-960e-c11b267d3618.png height = 350px width = 380px></p><br>

  - 이전 틱택토에서 컴퓨터가 한번 더 진행해서 컴퓨터의 턴인지 물어보고 승패여부 따지는 식으로 반복

### **배운 개념**

- `JSON.parse() / JSON.stringify()`

  ```javascript
  const monster1 = JSON.parse(JSON.stringify(monsterList[0])); // 참조방식과 복사가 있다. 얕은 복사, 깊은 복사.
  const monster2 = monsterList[0]; //객체 대입하면 참조.
  monster1.name = "새 몬스터";
  console.log(monsterList[0].name); //슬라임
  monster2.name = "새 몬스터";

  console.log(monsterList[0].name); //새 몬스터
  console.log(monsterList[0] === monster1); // false 깊은 복사
  console.log(monsterList[0] === monster2); // true 참조 관계
  ```

  실습코드에서 위 코드를 추가했을 떄, 기존 `monsterList`배열의 객체에서 정보를 일방적으로 가져다 써야하는 경우가 많을 것이다. <br>예를 들어 100중에 70정도를 서버의 데이터를 가져와서 쓰고 몇가지 일부만 서버에 데이터를 던져 업데이트해주는 것과 같다고 보면 된다.
  실습코드가 클라이언트라고 가정하면 `monsterList`의 배열은 특정 상황 외에는 업데이트를 하면 안되기에 참조방식의 변수핸들링을 해선 안되고 별도의 공간에 복사하여 의도치 않은 상호참조를 방지하고자 <u>얕은 복사(shallow copy)와 깊은 복사(deep copy)</u> 개념이 있다. 위 예제코드는 <u>깊은 복사</u> 개념을 사용한 예제이며 참고로, `JSON.parse(JSON.stringify())`방식은 깊은 복사방법 중 하나일 뿐이다.
  <br>
  그럼 얕은 복사는 어떻게 쓰는지 보자.

  ```javascript
  const monster3 = { ...monster[0] }; //객체 리터럴 복사.
  const arr = [...arr]; //배열 복사.
  ```

  중첩된 객체가 있을 때 가장 바깥 객체만 복사되고, 내부 객체는 참조 관계를 유지하는 복사다. 만약 객체 리터럴을 복사할 때 중괄호 내에 ...를 쓰면 되고 배열은 대괄호 내에 ...을 쓰면 된다.

- **_얕은 복사와 깊은 복사의 차이._** <br>
  얕은 복사는 외부만 복사 내부는 참조, 깊은 복사는 모두 참조관계가 끊기고 복사.

  ```javascript
  const a = [];
  const b = "hello";
  const c = {};
  const arr = [a, b, c];

  const arr1 = arr; // 참조관계
  arr1[1] = "hi"; //hi
  arr[1]; //hi

  const arr2 = [...arr]; //얕은 복사
  arr2[1] = "메롱"; //메롱
  arr[1]; //hi

  arr2[0].push(1); //1
  arr[0]; //1
  //얕은 복사는 배열([])은 복사하되(객체가 아닌 값들은 참조가 없기 때문에 복사가 된다.), a,b,c는 참조(객체기 때문)다. 대부분은 얕은 객체로 해결이 된다.

  //객체 내에 객체가 있는 경우는 깊은 복사를 해야하는 상황이 생길 수 있다.
  const arr3 = JSON.parse(JSON.stringify(arr));
  arr3[0].push(2);
  arr[0]; //1
  arr3[0]; //2
  ```

  덧붙여서 `JSON.parse(JSON.stringify(객체))`는 성능도 느리고 함수나 `Math`, `Date`같은 객체를 복사할 수 없다는 단점이 있다. 실무에서는 `lodash`같은 라이브러리를 사용하곤 한다. 추가로 저번에 공부한 `slice()`가 있으며 `concat()`도 있다.

- **`메서드`**

  - this<br>
    `this`는 자바스크립트에선 다른 언어와 다르다. 객체에서 `this`를 쓰면 해당 객체를 가리키는 기능을 하지만 이 상황은 `function()`형태에서만 적용이 된다. 하지만 `()=>`화살표함수를 쓰면 `this`는 `window`를 가리키게 된다. 엄밀히 말하면 `객체.메서드` 형태를 써야지만 `this`가 객체를 가리킬 수 있다.

- **`클래스`**
- 2015년 전엔 factory함수(패턴) 또는 생성자(new) 호출 방식으로 클래스를 구현했었다. new를 붙이지 않으면 this가 window를 가리키게 된다.
- 프로토타입을 써서 메서드를 제공했지만 따로 적용했었다.

  - 프로토타입이란?<br>
    `prototype`은 생성자 함수에 메서드를 추가할 때 `prototype`이라는 속성 안에 추가해야 한다. 그 속성 안에 추가한 메서드를 <u>프로토타입메서드</u>u>라고 한다. 이렇게 하면 팩토리패턴과 달리 메서드를 재사용할 수 있는 장점이 있지만 생성자 함수와 프로토타입 메서드가 하나로 묶여있지 않았다.

- 2015년 이후엔 class를 제공하기 시작했다. 여기서도 new 잊지말자 ~~컴파일러가 잡아주긴 한다~~

  ```javascript
  class Hero {
    constructor(game, name) {
      this.game = game;
      this.name = name;
      this.lev = 1;
      this.maxHp = 100;
      this.hp = 100;
      this.xp = 0;
      this.att = 10;
    }
    attack(target) {
      target.hp -= this.att;
    }
    heal(monster) {
      this.hp += 20;
      this.hp -= monster.att;
    }
  }
  ```

  - 위의 예제가 최신 문법(?)인데, 생성자와 메소드를 한데로 묶을 수 있어서 훨씬 Clean함이 느껴지고 실제로도 생성자를 호출할 때 매우 유용하다. 대신 설계가 중요하다. 이 뒤 부터는 `text-rpg-class.html` 실습파일에서 진행한다.

- **this**<br>
  `this`는 위치에 따라 가리키는 것이 수시로 바뀔 수 있다. 예를 들어 약속 중 하나인 `addEventListener`를 `function`형태로 쓴다면 함수 내부의 `this`는 `addEventListener`가 가리키는 메모리 공간(document)을 가리키게 되어 클래스 내의 `this`랑 다르게 가리키는 경우가 있다. 이러한 현상을 해결하기 위해 우리는 `arrowFunction`을 배웠다. `arrowFunction`을 적용하면 바깥의 `this`를 그대로 가져올 수 있는 이점이 있다. 그래서 `function()`과 `arrowFunction` 둘 다 상황에 맞게 사용하면 된다.<br>이게 익숙하지 않으면 `console.log(this)`를 해서 무엇을 가리키는지 수시로 확인하는 버릇을 가지자.<br>
  참고로 `this`는 함수가 호출될 때 결정이 되기 때문에 아래 실습 코드와 같이 세팅하면 문제가 없다.

  ```javascript
  start() {
          $gameMenu.addEventListener("submit", this.onGameMenuInput);
          $battleMenu.addEventListener("submit", this.onBattleMenuInput);
          this.changeScreen("game");
        }
        onGameMenuInput = (event) => {
          event.preventDefault();
          const input = event.target["menu-input"].value;
            this.changeScreen("battle");
          } else if (input === "2") {
          } else if (input === "3") {
          }
        };
        onBattleMenuInput = (event) => {
          event.preventDefault();
          const input = event.target["battle-input"].value;
          if (input === "1") {
          } else if (input === "2") {
          } else if (input === "3") {
          }
        };
  ```

  ```javascript
  document.addEventListener("click", () => {
    console.log(this); //window
  });
  ```

  함수 선언문일 때만 `document`가 나오는 이유는, `click`이벤트가 발생할 때 `addEventListener` 메서드가 콜백함수의 `this`를 `event.target`으로 바꿔서 호출하기 때문이다.
  <br>그러므로 함수 선언문의 `this`는 `bind`메서드를 사용해서 직접 바꿀 수 있다. 아래를 보자.

  ```javascript
  function a() {
    console.log(this);
  }
  a.bind(document)(); //document
  ```

  화살표 함수는 `bind`를 할 수 없어서 `this`가 바뀌지 않아 `window`가 그대로 나온다. 아래를 보자.

  ```javascript
  const b = () => {
    console.log(this);
  };
  b.bind(document)(); //window
  ```

- 클래스 상속

  - 만약 영웅과 몬스터가 아닌 보스몬스터 또는 에픽등급의 몬스터를 생성하고 싶으면 `class BossMonster` 또는 `class EpicMonster` 이런 식으로 새로운 클래스를 생성해야 할 것이다. 하지만 `Hero, Monster, BossMonster, EpicMonster`의 클래스는 **_공통의 속성_**을 지닌 것을 볼 수 있다.

  ```javascript
  class Hero {
    constructor(game, name) {
      this.game = game; //공통
      this.name = name; //공통
      this.lev = 1;
      this.maxHp = 100; //공통
      this.hp = 100; //공통
      this.xp = 0; //공통
      this.att = 10; //공통
    }
    attack(target) {
      //공통
      target.hp -= this.att;
    }
    heal(monster) {
      this.hp += 20;
      this.hp -= monster.att;
    }
    getXp(xp) {
      this.xp += xp;
      if (this.xp >= this.lev * 15) {
        //경험치를 다 채우면
        this.xp -= this.lev * 15;
        this.lev += 1;
        this.maxHp += 5;
        this.att += 5;
        this.hp = this.maxHp;
        this.game.showMessage(`레벨업! ${this.lev}.LV`);
      }
    }
  }
  class Monster {
    constructor(game, name, hp, att, xp) {
      this.game = game; //공통
      this.name = name; //공통
      this.maxHp = hp; //공통
      this.hp = hp; //공통
      this.xp = xp; //공통
      this.att = att; //공통
    }
    attack(target) {
      //공통
      target.hp -= this.att;
    }
  }
  ```

  공통의 속성을 따로 빼서 `Unit`이라는 클래스를 만들어보자.

  ```javascript
  class Unit {
    constructor(game, name, hp, att, xp) {
      this.game = game;
      this.name = name;
      this.maxHp = hp;
      this.hp = hp;
      this.xp = xp;
      this.att = att;
    }
    attack(target) {
      target.hp -= this.att;
    }
  }
  ```

  그리고 이 공통클래스는 `extends`를 사용하여 자식클래스인 `Monster`과 `Hero`에 상속할 수 있다. 결국 `Unit`은 `Hero`의 부모클래스인 셈이다. 방법은 이렇다.

  ```javascript
  class Hero extends Unit {
    constructor(game, name) {
      //super(game,name,100,10,0) 추가.
      this.game = game; //공통(제거)
      this.name = name; //공통(제거)
      this.lev = 1;
      this.maxHp = 100; //공통(제거)
      this.hp = 100; //공통(제거)
      this.xp = 0; //공통(제거)
      this.att = 10; //공통(제거)
    }
    attack(target) {
      //공통(제거)
      target.hp -= this.att;
    }
    heal(monster) {
      this.hp += 20;
      this.hp -= monster.att;
    }
    getXp(xp) {
      this.xp += xp;
      if (this.xp >= this.lev * 15) {
        this.xp -= this.lev * 15;
        this.lev += 1;
        this.maxHp += 5;
        this.att += 5;
        this.hp = this.maxHp;
        this.game.showMessage(`레벨업! ${this.lev}.LV`);
      }
    }
  }
  class Monster extends Unit {
    constructor(game, name, hp, att, xp) {
      //super(game,name,100,10,0) 추가.
      this.game = game; //공통(제거)
      this.name = name; //공통(제거)
      this.maxHp = hp; //공통(제거)
      this.hp = hp; //공통(제거)
      this.xp = xp; //공통(제거)
      this.att = att; //공통(제거)
    }
    attack(target) {
      //공통(제거)
      target.hp -= this.att;
    }
  }
  ```

  이렇게 상속받은 속성 중 공통속성을 모두 제거한 후에 `super()`를 사용하여 부모클래스인 `Unit`의 생성자를 호출한다. 정리하면 아래와 같다.

  ```javascript
  class Hero extends Unit {
    constructor(game, name) {
      super(game, name, 100, 10, 0);
      this.lev = 1;
    }
    heal(monster) {
      this.hp += 20;
      this.hp -= monster.att;
    }
    getXp(xp) {
      this.xp += xp;
      if (this.xp >= this.lev * 15) {
        this.xp -= this.lev * 15;
        this.lev += 1;
        this.maxHp += 5;
        this.att += 5;
        this.hp = this.maxHp;
        this.game.showMessage(`레벨업! ${this.lev}.LV`);
      }
    }
  }
  class Monster extends Unit {
    constructor(game, name, hp, att, xp) {
      super(game, name, hp, att, xp);
    }
  }
  ```

  추가설명하자면 자바스크립트는 다중 상속을 지원하지 않는 언어다.(하나의 클래스는 하나만 상속이 가능하다.) 역으로 정상작동하는 코드를 `분석`할 때, 해당 클래스에 없는데도 호출을 했다면 `부모클래스로 거슬러 올라가면서 메소드 또는 생성자를 찾아보면` 구조파악하는데 어렵지 않을 것이다.

### **느낀 점**

- 느낀 점이라기 보단 채찍질이다. 이것 저것 배우면서 커밋도 커밋이지만 개념 복습을 필수로 하자! 게으르게 하지 말고 꾸준하고 반복적으로 성실하게!!

  <br>
  <br>

## **틱택토 컴퓨터전**

### `2021.05.03 tictactoe-self.html`<br><br>

### **플로우**

<p align="center"><img src = https://user-images.githubusercontent.com/74204327/116888857-e85d4380-ac66-11eb-92b0-1893ad4ddef8.png height = 350px width = 430px></p><br>

- 이전 틱택토에서 컴퓨터가 한번 더 진행해서 컴퓨터의 턴인지 물어보고 승패여부 따지는 식으로 반복

### **배운 개념**

- `filter(()=>)` : 조건()에 맞는 데이터 필터링

  - 빈칸인 데이터를 찾아서 그 데이터 길이만큼 반복하여 랜덤함수로 랜덤값 추출할 수 있게 한다.

- 핵심은 timeOut실행도중 클릭 못하도록 처리하는 것(clickable 플레그 사용)이랑 게임이 끝났는데 클릭이 되도록 막는 것이다.($table.removeEventLisener('click', callback))

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
