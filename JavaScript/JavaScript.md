#### JavaScript

##### 개요

- 프로그래밍언어로서 기본 뼈대를 이루는 ECMAScript와 브라우저가 별도 지원하는 클라이언트 사이드 Web API ( DOM, Canvas, Fetch, SVG 등 )을 아우르는 개념이다.
- HTML, CSS와 함께 웹을 구성하는 요소 중 하나로 웹 브라우저에서 동작하는 유일한 프로그래밍 언어이다.
- 개발자가 별도의 컴파일 작업을 수행하지 않는 인터프리터 언어이다.
- 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어이다.
- IE처럼 ES6를 지원하지 않는 경우도 있어 ES6로 구현된 소스코드의 경우, babel과 같은 트랜스파일러를 사용할 필요가 있다.
- 모듈 import / export 같은 경우, 대부분의 브라우저가 지원하고 있지 않기 때문에, Webpack과 같은 모듈 번들러를 사용해야 한다.

##### 자바스크립트 엔진

- 모든 브라우저와 Node.js는 자바스크립트를 해석하고 실행할 수 있는 자바스크립트 엔진을 내장하고 있다.
- ECMAScript 이외에 추가적으로 제공하는 기능은 호환되지 않는다.
  - Node.js 가 브라우저의 DOM API 사용 불가. 브라우저가 Node.js의 File 시스템 사용불가.
- Node.js
  - 크롬 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경이다.
  - 주로 서버 사이드 애플리케이션 개발에 사용되며 이에 필요한 모듈, 파일 시스템, HTTP 등 빌트인 API를 제공한다.
  - 데이터를 실시간 처리하여 빈번한 I/O가 발생하는 SPA에 적합하다.
  - 하지만 CPU 사용률이 높은 애플리케이션에는 권장하지 않는다.
  - 프런트엔드 영역의 다양한 도구나 라이브러리도 Node.js 환경에서 작동한다.
  - npm은 자바스크립트 패키지 매니저로 Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI를 제공한다.

##### 변수

- 변수, 상수

- let, const - 블록 유효범위 변수

- var

  - 같은 이름으로 여러번 선언할 수 있다.
  - scope가 다르다.

- 리터럴

  - 소스코드 안에서 직접 만들어 낸 상수 값 자체를 말하며 값을 구성하는 최소 단위
  - 값은 리터럴 표기법으로 표현된다.

  ```javascript
  // 숫자 리터럴
  10.50
  1001

  // 문자열 리터럴
  'Hello'
  "World"

  // 불리언 리터럴
  true
  false

  // null 리터럴
  null

  // undefined 리터럴
  undefined

  // 객체 리터럴
  { name: 'Lee', gender: 'male' }

  // 배열 리터럴
  [ 1, 2, 3 ]

  // 정규표현식 리터럴
  /ab+c/

  // 함수 리터럴
  function() {}
  ```

##### 데이터 타입 ( 7개 )

- Boolean
- Null
  - 의도적으로 값이 없음을 가리키는 '객체' 타입의 객체
- Undefined
  - 초기화 되지 않은 값. 아직 어떤 값도 주어지지 않은 변수임을 가리키는 ''정의되지 않음'' 타입의 객체
- Number
  - 이중정밀도 64비트 형식 IEEE 754 값
  - Math 내장 객체
  - parseInt(), parseFloat()
  - NaN
- String
  - 유니코드 문자들이 연결되어 만들어짐
- Symbol
- Object
  - 함수
  - 배열
  - 날짜
  - 정규식

##### 제어구조

- for ~ in - object의 항목으로 작업을 실행한다.
- for ~ of - array의 value로 작업을 실행한다.
- **단축평가 논리**. && / || - 첫번째 식을 평가한 결과( Truty냐 Falthy냐)에 따라 두번째 식을 평가.
- 논리연산자의 연산 순서 - NOT > AND > OR

##### 변수

- 변수는 값의 위치(주소)를 기억하는 저장소이다.

- 변수에는 7개의 데이터 타입이 있으며, 크게 원시 타입과 객체 타입으로 구분할 수 있다.

- 원시 타입

  - 변경 불가능한 값 ( immutable value )이며 pass-by-value( 값에 의한 전달 )이다.

  - 따라서 값을 재할당하면, 주소가 변경된다.

  - number

    - 배정밀도 64비트 부동소수점 형을 따른다
    - 모든 수를 실수로 처리한다.
    - 2진수, 8진수, 16진수 리터럴 모두 표기만 다를 뿐 같은 값으로 처리한다.
    - 정수로 표시된다해도 사실은 실수이며, 정수로 표시되는 수끼리 나누더라도 실수가 나올 수 있다.

  - undefined

    - 선언되고 값이 할당되기 전에 빈 상태로 내려버두는 것이 아니라 자바스크립트 엔진이 undefined로 초기화한다.
    - 즉, 개발자가 의도적으로 할당한 값이 아니라 자바스크립트 엔진에 의해 초기화된 값이다.
    - 따라서 개발자가 의도적으로 undefined를 할당하는 것은 권장되지 않는다.
    - 변수의 값이 없다는 것을 명시하고 싶은 경우에는 null을 할당한다.

  - null

    - 의도적으로 변수에 값이 없다는 것을 명시할 때 사용한다.
    - 변수가 기억하는 메모리 어드레스의 참조 정보를 제거하는 것을 의미한다.

  - symbol

    - 이름의 충돌 위험이 없는 유일한 객체의 프로퍼티를 만들기 위해 사용한다.
    - 심볼은 Symbol 함수를 호출해 생성하며, 이때 생성된 심볼 값은 다른 심볼 값들과 다른 유일한 심볼 값이다.
    - Symbol() 함수에는 문자열을 인자로 전달할 수 있으며, 이 문자열은 Symbol 생성에 어떠한 영향을 주지 않고, 단지 생성된 Symbol에 대한 설명으로 디버깅 용도로만 사용된다.

    ```javascript
    const obj = {};

    const mySymbol = Symbol("mySymbol");
    obj[mySymbol] = 123;

    console.log(obj); // { [Symbol(mySymbol)]: 123 }
    console.log(obj[mySymbol]); // 123
    ```

##### 객체

- 원시 타입을 제외한 나머지 값들(함수, 배열, 정규표현식 등)은 전부 객체이다.
- 객체는 pass-by-reference(참조에 의한 전달) 방식으로 전달된다.

```javascript
// Pass-by-reference
var foo = {
  val: 10,
};

var bar = foo;
console.log(foo.val, bar.val); // 10 10
console.log(foo === bar); // true

bar.val = 20;
console.log(foo.val, bar.val); // 20 20
console.log(foo === bar); // true
```

- 키(이름)-값 쌍(name-value pairs)으로 구성된 프로퍼티의 집합이다.
- name은 JavaScript 문자열만 가능
- value는 객체 포함 어떤 JavaScript 값도 가능.
- 프로퍼티의 값이 함수일 경우, 일반 함수와 구분하기 위해 메소드라 부른다.
- 즉, 객체는 데이터를 의미하는 프로퍼티와 데이터를 참조하고 조작할 수 있는 동작을 의미하는 메소드로 구성된 집합이다.
- 자바스크립트의 객체는 객체지향의 상속을 구현하기 위해 '프로토타입'이라고 불리는 객체의 프로퍼티와 메소드를 상속받을 수 있다.
- 객체의 생성은 리터럴 구문과 Object 생성자 함수를 사용하여 생성할 수 있다.

```javascript
// 의미적으로 동일
var obj = new Object(); // new 키워드와 함께 사용하여 생성자함수로 객체 생성.
var obj = {}; // 객체 리터럴 구문으로 더 편리하다.
// 사실 빌트인 함수인 Object 생성자 함수로 객체를 생성하는 것을 단순화시킨 축약 표현이다.
// 자바스크립트 엔진은 리터럴 구문을 만나면 내부적으로 생성자 함수를 사용하여 객체를 생성한다.
```

- 객체 프로토 타입, 프로토타입의 인스턴스 생성

```javascript
// 객체를 정의. 생성자 함수를 만드는 것과 같다.
// 생성자 함수는 일반적으로 대문자로 시작한다.
function Person(name, age) {
  this.name = name; // 프로퍼티, 메소드 앞의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.age = age; // this에 연결되어 있는 프로퍼티와 메소드는 public(외부에서 참조 가능)하다.
  var married = true; // 이에 비해, 일반 변수는 private(외부에서 참조 불가능)하다.
}
// 인스턴스 생성
var you = new Person("You", 24);
```

- dot 표기법 / bracket 표기법

```javascript
var name = obj.name; // dot 표기법
obj["name"] = "Simon"; // bracket 표기법
```

- 배열

  - array.length는 배열에 들어있는 항복 수를 반드시 반영하지는 않는다.
  - 최대 인덱스에 하나를 더한 값일 뿐이다.
  - ES2015(ES6)는 배열과 같은 **이터러블 객체**를 위해 좀더 간결한 **for...of 루프**를 소개하였다.
  - **for...in 루프**의 경우, 배열 요소가 아닌 **배열 인덱스**를 반복한다.
  - 배열 내장함수
    - **Array.forEach()**는 주어진 함수( 콜백함수 - 함수형태의 파라미터 )를 배열 요소 각각에 대해 실행한다.
    - **Array.map(함수)**는 주어진 함수를 실행한 결과를 새로운 배열로 반환한다.
    - **Array.filter(조건)** 특정조건에 맞는 값들만 추출하여 새로운 배열로 반환한다.
    - **Array.reduce((누적, 현재, 인덱스, 배열) => {})** - 각 원소의 이전 실행값의 활용이 가능하다.
    - Array.indexOf('value') / Array.findIndex(조건) - 각 원소에 대해 실행 후 조건에 맞는 값을 반환한다.
    - Array.find(조건) - findIndex와 비슷하나 index가 아닌 값 자체를 반환한다.
    - Array.splice() / Array.slice() - slice는 잘라낸 새로운 배열을 반환한다.
    - Array.shift() / Array.pop() - 첫번째 / 마지막 원소 추출, 기존 배열 수정
    - Array.unshift() - 배열의 맨 앞에 새 원소 추가
    - arr1.concat(arr2) - 여러개의 배열을 합친 새로운 배열을 반환한다.
    - Array.join() - 배열안의 값들을 합쳐 문자열로 반환한다.

- 함수

  - return 문은 언제나 값을 돌려주고 함수의 실행을 끝내는데 사용될 수 있다.
  - return 문이 없으면 ( 혹은 값이 없는 return이 사용되면 ) undefined를 리턴한다.
  - 함수가 원하는 매개변수를 안주거나 더 줄 수 있으며, 그럴 경우 undefined가 되거나 무시된다.
  - 주어진 매개변수에 대해서 arguments로 접근할 수 있다.

  ```javascript
  function avg() {
    var sum = 0;
    for (var i = 0, j = arguments.length; i < j; i++) {
      sum += arguments[i];
    }
    return sum / arguments.length;
  }
  avg(2, 3, 4, 5); // 3.5
  ```

  - Rest파라미터 문법으로 대체 가능하다.

  ```javascript
  function avg(...args) {
    var sum = 0;
    for (let value of args) {
      sum += value;
    }
    return sum / arr.length;
  }
  avg(2, 3, 4, 5); // 3.5
  ```

  - this

  - new

  - prototype

    - JavaScript의 빌트인 객체의 prototype에도 추가가 가능하다.

    ```javascript
    var s = "Simon";
    s.reversed(); // TypeError on line 1: s.reversed is not a function

    String.prototype.reversed = function () {
      var r = "";
      for (var i = this.length - 1; i >= 0; i--) {
        r += this[i];
      }
      return r;
    };

    s.reversed(); // nomiS
    ```

  - 내장함수

    - 함수 내에 다른 함수를 선언할 수 있다.
    - 이 경우 부모 함수에서 선언된 변수를 사용할 수 있다.
    - 전역 범위 함수의 갯수를 늘리지 않도록 하는 면에서 좋다.
    - 전역 변수를 사용하지 않도록 하는 면에서 좋다.

- 객체 비구조화 할당

  - 객체 구조 분해라고 불리기도 한다.

  - 객체 안의 값들을 편하게 풀어 놓는 것(할당 하는 것).

    ```javascript
    const { alias, name, actor } = hero;
    ```

  - 배열에서도 비구조화 할당이 가능한다.

    ```javascript
    const array = [1];
    const [one, two = 2] = array;
    ```

* 객체 안에 함수를 넣을 때, 화살표 함수로 선언한다면 제대로 동작하지 않는다.

```javascript
const dog = {
  name: "멍멍이",
  sound: "멍멍!",
  say: () => {
    console.log(this.sound);
  },
};
// TypeError: Cannot read property 'sound' of undefined.
// funtion으로 선언한 함수는 this가 제대로 자신이 속한 객체를 가르키게 되는데, 화살표 함수는 그렇지 않다.
```

- 객체 안에 Getter, Setter 함수를 지정할 수 있다.
  - 실제 변수는 \_a 이지만, 직접 접근하지 않고 a를 조회해서 접근한다.
  - 이를 통해 캡슐화의 이점인 정보 은닉, 정보 보호가 가능하다.

```javascript
const numbers = {
  _a: 1,
  _b: 2,
  sum: 3,
  calculate() {
    console.log('calculate');
    this.sum = this._a + this._b;
  },
  get a() {
    return this._a;
  }
  set a(value) {
    console.log('a가 바뀝니다.');
    this._a = value;
    this.calculate();
  }
};
console.log(numbers.sum); // 조회하는 것만으로, 함수가 실행된다.
numbers.a = 5; // 조회하는 것만으로 Setter 함수가 실행된다.
```

```javascript
class Time {
  constructor(start, end) {
    this._start = start;
    this._end = end;
    this._duration = end - start;
  }
  set start(newStart) {
    this._start = newStart;
    this._duration = this._end - this._start;
  }
  get start() {
    return this._start;
  }
}
const time = new Time(0, 20);
time.start = 5;
// time._start로 적접 접근하면 _duration은 수정되지 않는다.
// 즉, 변수 관계에 있어서, 일관성을 깨뜨리게 된다.
// Getter와 Setter 사용을 통해 일관성을 유지하고, 코드의 가독성을 높일 수 있다.
```

##### Truthy, Falsy

- undefined와 null은 Falsy한 값이다. Falsy한 값에 느낌표를 붙여주면 true로 전환된다.
- 그 외에, 0, ' ', NaN 등이 있다.

```javascript
persion === undefined || person === null; // !person이랑 같다.
```

- []는 Truthy 한 값이다.

```javascript
console.log(!3);
console.log(!"hello");
console.log(!["array?"]);
console.log(![]);
console.log(!{ value: 1 });
```

##### spread / rest

- ...spread

  - spread 안에 있는 속성들을 모두 풀어 놓는 것이다.
  - 객체 혹은 배열을 펼칠 수 있다.

- ...rest

  - 객체, 배열, 그리고 함수의 파라미터에서 사용이 가능하다.

  - 비구조화 할당 문법과 함께 사용된다.

  - 사용할 때는 주로 rest라는 키워드와 함께 사용된다.

    ```javascript
    const { color, ...rest } = purpleCuteSlime;
    ```

  - 비구조화 할당을 통하여 원하는 값을 밖으로 꺼내고, 나머지 값을 rest 안에 집어 넣는다.

  - 나머지를 넣는 것이기 때문에 ...rest가 먼저 들어갈 수는 없다.

  - 함수의 파라미터가 몇 개인지 모르는 상황에서 유용하게 쓰일 수 있다.

    ```javascript
    function sum(...rest) {
      return rest;
    }
    const result = sum(1, 2, 3, 4, 5, 6);
    // rest는 파라미터들로 이루어진 배열이다.
    ```

##### 객체 생성자

- 객체 생성자 new
- 프로토 타입
- 객체 생성자 상속 call
- 클래스

##### 클래스

- 클래스 내부함수를 메서드라 한다.
- 메서드를 만들면 자동으로 prototype으로 등록이 된다.
- extends를 통해 쉽게 상속이 가능하다.
- constructor에서 super() 함수는 상속받은 클래스의 생성자를 가르킨다.

##### 클로져 ( Closures )

- 함수가 실행될 때는 언제나 '범위' 객체가 생성되어 해당 함수내에서 생성된 지역 변수를 여기에 저장한다.
- 전역객체와 달리 범위객체는 함수가 실행될 때마다 새로운 범위 객체가 생성된다.
- 또한, 브라우저에서 window로 접근 가능한 전역 객체와 달리, 범위 객체는 JavaScript 코드에서 직접적으로 엑세스 할 수 없다.
- 클로져는 함수와 함수에 의해 생성되는 범위 객체를 함께 지칭하는 용어이다.
- 클로져는 상태를 저장할 수 있도록 허용한다.

##### Scope

- var 키워드의 특징
  - 함수 레벨 스코프 - 전역 함수 외부에서 생성한 변수는 모두 전역 변수이다.
  - var 키워드 생략 허용 - 암묵적 전역 변수를 양산할 가능성이 크다.
  - 변수 중복 선언 허용 - 의도하지 않은 변수 값의 변경이 일어날 가능성이 크다.
  - 변수 호이스팅 - 변수를 선언하기 이전에 참조할 수 있다
- 전역 변수로 인한 문제

  - 유효 범위 ( Scope )가 넓어서 어디에서 어떻게 사용될 것인지 파익하기 힘들다.
  - 비순수 함수 ( impure function )에 의해 의도하지 않게 변경될 수도 있어서 복잡성을 증가시킬 수 있다.
  - ES6는 이러한 var 키워드의 단점을 보완하기 위해 let과 const 키워드를 도입하였다.

- Global(전역) Scope

  - 코드의 모든 범위에서 사용 가능

- Function(함수) Scope

  - 함수 안에서만 사용이 가능
  - var는 Function 스코프로 선언되기 때문에, if 블록 내부에서 선언한 값이 블록 외부의 값에도 영향을 미친다.

- Block(블록) Scope

  - if, for, switch 등 특정 블록 내부에서만 사용이 가능
  - let, const는 Block 스코프로 블록 내에서의 선언이 블록 외부에 영향을 미치지 않는다.

- **Hoisting**

  - hoist - to lift something heavy, sometimes using ropes or a machine

  - 아직 선언되지 않은 **모든 선언( 함수/변수/클래스 )**를 '끌어올려서' 사용하는 작동 방식

    ```javascript
    myFunction();

    function myFunction() {
      console.log("hello world!");
    }
    ```

  - var의 경우, 변수도 hoisting이 작동한다.

    ```javascript
    console.log(number);
    var number = 2;
    ```

    ```javascript
    // 자바스크립트 엔진이 해석할 때는 아래와 같이 받아들임.
    var number;
    console.log(number);
    number = 2;
    ```

  - let, const의 경우 hoisting이 되나, 스코프의 시작에서 변수의 선언까지 일시적인 사각지대(TDZ) 가 생성되어, 초기화 전에는 엑세스할 수 없다.

  - Hoisting은 방지하는 것이 가독성, 유지보수 측면에서 좋다.

  - 변수의 생성

    - 선언 단계 - 변수를 실행 컨텍스트의 변수 객체에 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다.
    - 초기화 단계 - 변수 객체에 등록된 변수를 위한 공간을 메모리에 확보한다. 이 단계에서 변수는 undefined로 초기화된다.
    - 할당 단계 - undefined로 초기화된 변수에 실제 값을 할당한다.

  - var 키워드로 선언된 변수는 선언 단계와 초기화 단계가 한 번에 이루어진다. 즉, 선언과 함께 undefined로 초기화 되는 것이다.

  - 반면, let 키워드로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다. 즉, 초기화 단계는 변수 선언문에 도달했을 때 이루어진다. 따라서 초기화 이전에 변수에 접근하려고 하면 참조 에러가 발생한다.

  - 여기서 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 **'일시적 사각지대(TDZ)'**이라고 부른다.

    - 스코프 시작 부분에서 선언 단계는 실행된다. 다만, 초기화 단계(메모리 할당)가 안 이루어진 것이다.

  ```javascript
  let foo = 1; // 전역 변수

  {
    console.log(foo); // ReferenceError: foo is not defined
    let foo = 2; // 지역 변수
  }
  // hoisting이 안 일어난 것처럼 보이나, hoisting이 일어났기 때문에 참조 에러가 발생한 것이다.
  ```

##### Prototype 객체

- Prototype은 생성자 함수에 의해 생성된 각각의 객체에 공유 프로퍼티를 제공하기 위해 사용한다.

```javascript
var student = {
  name: "Lee",
  score: 90,
};
// student에는 hasOwnProperty 메소드가 없지만 아래 구문은 동작한다.
// _proto_ 즉, Prototype에 hasOwnProperty 라는 메소드가 있기 때문
console.log(student.hasOwnProperty("name")); // true
```

- 자바스크립트의 모든 객체는 [[Prototype]]이라는 인터널 슬롯을 가진다.
- [[Prototype]]의 값은 Prototype 객체이며 ** proto ** 프로퍼티로 접근할 수 있으며, 내부적으로 Object.getPrototypeOf가 호출되어 프로토 타입 객체를 반환한다.

##### Strict 모드

- 암묵적 전역변수를 허용하지 않는다.
- 변수, 함수, 매개변수의 삭제를 허용하지 않는다.
- 중복된 파라미터 이름을 허용하지 않는다.
- 생성자 함수가 아닌 일반 함수에서는 this에 undefined가 호출된다.

##### this

- 자바스크립트의 함수는 호출될 때, 매개변수로 전달되는 인자값 외에, arguments 객체와 this를 암묵적으로 전달받는다.

- Java에서 this는 인스턴스 자신을 가리키는 참조변수이다.

- JavaScript는 해당 함수 호출 방식에 따라 this에 바인딩 되는 객체가 달라진다.

- 함수 호출 방식

  - 함수 호출
  - 메소드 호출
  - 생성자 함수 호출
  - apply / call / bind 호출

  ```javascript
  var foo = function () {
    console.dir(this);
  };

  // 1. 함수 호출
  foo(); // window
  // window.foo();

  // 2. 메소드 호출
  var obj = { foo: foo };
  obj.foo(); // obj

  // 3. 생성자 함수 호출
  var instance = new foo(); // instance

  // 4. apply/call/bind 호출
  var bar = { name: "bar" };
  foo.call(bar); // bar
  foo.apply(bar); // bar
  foo.bind(bar)(); // bar
  ```

- 콜백 함수 내부의 this는 전역 객체 window를 가리킨다.

  - **생성자 함수와 객체의 메소드, 이벤트 리스너를 제외**한 모든 함수(내부함수, 콜백함수 포함) 내부의 this는 전역객체를 가리킨다.
  - 콜백 함수 내부의 this가 메소드를 호출한 객체(생성자 함수의 인스턴스)를 가리키게 하려면 3가지 방법이 있다. binding 작업이 따로 필요하다.
    - 밖에서 this를 연결한 변수 만들기 / var that = this;
    - 콜백 함수 끝에서 this로 묶어주기 / (콜백함수, this)
    - bind() 함수로 this를 바인딩하기 / 함수.bind(this)
  - 화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정된다.
  - 화살표 함수의 this는 언제나 상위 스코프의 this를 가리킨다. 이를 Lexical this라고 한다.
  - 화살표 함수는 call, apply, bind 메소드를 사용하여 this를 변경할 수 없다.
  - 화살표 함수로 메소드를 정의할 경우 혹은 prototype에 메소드를 할당하는 경우, 그 안에서 this는 전역 객체 window를 가리키게 되므로 피하는 것이 좋다.

  ```javascript
  // Bad
  const person = {
    name: "Lee",
    sayHi: () => console.log(`Hi ${this.name}`),
  };

  person.sayHi(); // Hi undefined

  // Bad
  const person = {
    name: "Lee",
  };

  Object.prototype.sayHi = () => console.log(`Hi ${this.name}`);

  person.sayHi(); // Hi undefined
  ```

  - 화살표 함수는 prototype 프로퍼티를 가지지 않으므로 생성자 함수로 사용할 수 없다.

##### 프로미스

- 비동기처리를 위한 하나의 패턴으로 콜백함수를 사용했으나, 가독성이 나쁘고 에러의 예외처리가 곤란하다.
- 이를 해결하기 위해 다른 패턴으로 Promise를 도입하였다.
- Promise 생성자 함수는 비동기 작업을 수행할 콜백함수를 인자로 전달받고, 이 콜백함수는 resolve와 reject 함수를 인자로 전달받는다.
- 이 비동기 함수는 Promise 객체를 반환한다. 이를 처리 하기위한 후속처리 메소드가 then, catch이다.

##### 실행 컨텍스트

- 실행 가능한 코드가 실행되기 위해 필요한 환경
- 자바스크립트 엔진은 코드를 실행하기 위해서 실행에 필요한 여러가지 정보를 알고 있어야 한다.
  - 변수, 함수 선언, 스코프, this
- 이와 같이 실행에 필요한 정보를 형상화하고 구분하기 위해 자바스크립트 엔진은 실행 컨텍스트를 물리적 객체의 형태로 관리한다.

##### 클로저

- 함수와 그 함수가 선언되었을 때의 렉시컬 환경과의 조합
- 외부함수의 실행이 끝나도 내부함수에서 외부함수의 변수를 사용할 수 있도록 하는 것

##### 동일 출처 원칙

- 보안상의 이유로 다른 도메인으로의 요청이 제한된다.
- 이를 피하기 위한 방법이 3가지 있다.
  - 웹서버의 프록시 파일
  - JSONP
  - CORS
    - HTTP 헤더에 추가적으로 정보를 추가하여 브라우저와 서버가 서로 통신해야한다는 사실을 알게하는 방법
    - 서버에 HTTP 헤더를 설정해줘야한다.
    - CORS 패키지를 사용하면 간단하게 구현할 수 있다.

### JavaScript 배열은 진짜 배열이 아니다.

- 배열은 연속적인 메모리 로케이션의 묶음을 사용한다.
- 따라서 특정 값을 찾기가 용이하다.
  ( 어느 메모리 블록에 저장되어있는지 찾기가 쉬움 )
- 반면, JavaScript의 배열은 hash-map 이다.
  Linked List를 통해서 구현된다. ( ??? )
- 특정 값을 찾으려면 메모리 시작점부터 원하는 값까지 탐색해 나가야 한다.

### JavaScript 배열의 진화

- 최근 JavaScript 엔진의 경우, 모든 요소가 동일한 타입을 가진 배열의 경우, 연속적으로 메모리를 할당한다.
- ES6 이후, Typed Array가 추가되었으며, 이를 통해 ArrayBuffer를 사용할 수 있게 되었다.
- ArrayBuffer는 contiguous 메모리 블록을 제공하고, 이것을 마음대로 다룰 수 있게 해준다.
- ArrayBuffer를 사용한 Typed Array는 기존 JS의 Array보다 몇 배나 월등한 처리 속도를 가진다.

http://voidcanvas.com/javascript-array-evolution-performance/

#### ...연산자

spred와 rest 두가지로 쓰인다.
파라미터로 쓰일 경우 rest 연산자로 파라미터들을 하나의 values 배열로 묶는다.
( 일반적으로 쓰는 args는 유사 배열 객체이다. 그래서 array에게 기대되는 foreach, map 등의 메서드가 없다. )
화살표 함수에서 유용하게 사용된다.
( 화살표 함수에서는 args 객체를 쓸 수 없다.)

Iterable한 객체에 사용하면 spread의 역할을 한다.
객체를 풀어 놓을 때 사용한다.

#### Arrow Function

함수를 축약하여 사용하는 것이다.
함수를 값처럼 사용할 수 있다.
화살표 함수에서 this는 화살표 함수 밖의 this와 같다.

```javascript
const exponent = (exp) => (base) => base ** exp;
// exp를 받아서 base를 exp 만큼 거듭 제곱한다.
// 즉, 화살표 함수로 함수값을 가지고 있는 객체를 만들 수도 있는 것이다.

const square = exponent(2);
const cube = exponent(3);
const powerOf4 = exponent(4);

square(5); //25
cube(5); //125
powerOf4(5); //625

// 만일 여기서 this를 쓰면 전역을 가르킨다.
```
