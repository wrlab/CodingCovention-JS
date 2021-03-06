# WRL JavaScript 스타일 가이드

## 원문
 - [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
 - [Airbnb JavaScript Style Guide(en)](https://github.com/airbnb/javascript)
 - [Airbnb JavaScript Style Guide(ko)](https://github.com/tipjs/javascript-style-guide)

## 목차

1. [개요](#markdown-header-1)
1. [소스 파일](#markdown-header-2)
    1. [파일 이름](#markdown-header-21)
    1. [파일 인코딩](#markdown-header-22)
    1. [특수 문자](#markdown-header-23)
1. [네이밍](#markdown-header-3)
    1. [기본 규칙](#markdown-header-31)
    1. [식별자 타입에 따른 규칙](#markdown-header-32)
        1. [Object](#markdown-header-321-object)
        1. [Class](#markdown-header-322-class)
        1. [Property](#markdown-header-323-property)
        1. [Constant](#markdown-header-324-constant)
        1. [Parameter](#markdown-header-325-parameter)
1. [Code Formatting](#markdown-header-4-code-formatting)
    1. [중괄호](#markdown-header-41)
    1. [들여쓰기](#markdown-header-42)
    1. [구문](#markdown-header-43)
    1. [Line-wrapping](#markdown-header-44-line-wrapping)
    1. [공백](#markdown-header-45)
    1. [소괄호를 이용한 그루핑](#markdown-header-46)
    1. [콤마](#markdown-header-47)
1. [JavaScript 코드 작성](#markdown-header-5-javascript)
    1. [변수 선언](#markdown-header-51)
    1. [형](#markdown-header-52)
    1. [Array](#markdown-header-53-array)
    1. [Object](#markdown-header-54-object)
    1. [Class](#markdown-header-55-class)
    1. [Function](#markdown-header-56-function)
    1. [String](#markdown-header-57-string)
    1. [형변환](#markdown-header-58)
    1. [제어 구문](#markdown-header-59)
        1. [For loops](#markdown-header-591-for-loops)
        1. [Error Handling](#markdown-header-592-error-handling)
        1. [비교 연산](#markdown-header-593)
        1. [Switch](#markdown-header-594-switch)
    1. [코멘트](#markdown-header-510)
    1. [Destructuring](#markdown-header-511)
    1. [사용하면 안되는 것들](#markdown-header-512)
        1. [with](#markdown-header-5121-with)
        1. [eval](#markdown-header-5122-eval)
        1. [Function](#markdown-header-5123-function)
        1. [기본형의 래퍼 클래스에 대한 new 키워드 사용](#markdown-header-5124-new)


## 1 개요
  본 문서에서는 ECMAScript6에서 추가되거나 수정된 사항들을 포함하여 **JavaScript 코드 작성시 지켜야 할 코드 작성 규칙**을 정의 합니다.

## 2 소스 파일
### 2.1 파일 이름
  - 소스 파일 이름은 기본적으로 camelCase 명명규칙을 따르며, `.js`확장자로 끝납니다.

        main.js
        duckFlying.js
        implementAlgorithm.js

  - Default export가 함수일 경우, camelCase 명명규칙을 따릅니다. 파일명은 함수명과 동일해야 합니다.

        :::javascript
        function makeStyleGuide() {
          // function implementation...
        }

        // 파일 이름
        makeStyleGuide.js

  - 파일 1개의 클래스로 export 하는 경우, PascalCase 명명규칙을 따릅니다. 파일명은 클래스명과 일치시킵니다.

        :::javascript
        // 파일 내용
        class CheckBox {
          // class implementation...
        }

        // 파일 이름
        CheckBox.js

  - singleton / function library / 빈오브젝트를 export 하는 경우, PascalCase 명명규칙을 따릅니다.

        :::javascript
        const JavascriptStyleGuide = {
          es6: {
          },
        };

        // 파일 이름
        JavascriptStyleGuide.js;


### 2.2 파일 인코딩
  - 소스 파일의 문자 인코딩 방식은 **UTF-8**을 사용합니다.

### 2.3 특수 문자
  - 공백 문자는 오직 ASCII horizontal space character (0x20) 만을 사용합니다. 이는 **들여쓰기로 탭을 사용하지 않음**을 의미합니다. [작업 환경에서 탭을 사용하지 말라는 의미는 아닙니다.](#markdown-header-42)
  - ASCII로 표현가능한 **이스케이프 문자** (`\'`, `\"`, `\\`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`) 는 숫자 기반의 이스케이프 표현 (e.g `\x0a`, `\u000a`, or `\u{a}`) 을 사용하지 않습니다.
  - Non-ASCII 문자는 **유니코드 이스케이프**를 사용합니다. 이 때 주석으로 어떤 문자인지 설명을 달아줍니다.

    | Example                                            | Discussion                                                         |
    | -------------------------------------------------- | ------------------------------------------------------------------ |
    | const units = 'μs';                                | Best, 주석이 필요 없이 명확함                                        |
    | const units = '\u03bcs'; // 'μs'                   | Allowed                                                            |
    | const units = '\u03bcs'; // Greek letter mu, 's'   | Allowed                                                            |
    | const units = '\u03bcs';                           | Poor, '\u03bcs'가 무슨 문자인지 알 수 없음                           |
    | return '\ufeff' + content; // byte order mark      | Good, 출력 불가능한 문자 표현을 위해 이스케이프하였으며, 주석이 적절함  |

## 3 네이밍
### 3.1 기본 규칙
  - 모든 식별자는 ASCII 문자와 숫자, 밑줄, 달러 기호를 이용하여 만듭니다.
  - 축약어는 흔히 알려진 것만을 사용할 수 있으며, 식별자의 의미를 쉽게 파악할 수 있게 descriptive한 이름을 사용합니다.

        :::javascript
        // bad
        n                     // 의미를 알 수 없음
        nErr                  // 너무 모호한 축약
        nCompConns            // 너무 모호한 축약
        wgcConnections        // 축약어 wgc의 의미를 알 수 없음
        pcReader              // pc가 무엇의 축약인지 알 수 없음
        kSecondsPerDay        // 헝가리안 명명규칙을 사용하지 말 것

        // good
        priceCountReader      // 축약어를 사용하지 않음
        numErrors             // "num"은 흔히 알려진 축약어
        numDnsConnections     // 거의 모든 사람들이 "DNS"의 의미를 이해하고 있음


### 3.2 식별자 타입에 따른 규칙
#### 3.2.1 Object
  - Object, Function, instance는 camelCase 명명규칙을 따릅니다.

        :::javascript
        // bad
        const OBJEcttsssss = {};
        const this_is_my_object = {};
        function c() {}

        // good
        const thisIsMyObject = {};
        function thisIsMyFunction() {}


#### 3.2.2 Class
  - Class, interface, Package, record, typedef name은 PascalCase 명명규칙을 따릅니다.

        :::javascript
        // bad
        const roundHat = require('RoundHat');

        function user(options) {
          this.name = options.name;
        }

        const bad = new user({
          name: 'nope',
        });

        // good
        const RoundHat = require('RoundHat');

        class User {
          constructor(options) {
            this.name = options.name;
          }
        }

        const good = new User({
          name: 'yup',
        });


#### 3.2.3 Property
  - Property는 camelCase 명명규칙을 따릅니다.
  - private의 경우 밑줄 기호(`_`)로 시작합니다.

        :::javascript
        // bad
        this.__firstName__ = 'Panda';
        this.firstName_ = 'Panda';

        // good
        this._firstName = 'Panda';


#### 3.2.4 Constant
  - 대문자와 밑줄 기호로만 짓습니다.

        :::javascript
        const NUMBER_OF_PEOPLE = 5;
        let Math = {
          get PI() { return 3.14; }
        }


#### 3.2.5 Parameter
  - Parameter는 camelCase 명명규칙을 따릅니다.

        :::javascript
        function get(parmeterUrl, parameterBody, ...args) {
          ...
        }


## 4 Code Formatting
### 4.1 중괄호
  - **모든 제어구문**(e.g. `if`, `else`, `for`, `do`, `while`, 등)에서 중괄호를 반드시 사용합니다.
  - 중괄호 앞에는 **스페이스 1개**를 넣습니다.

        :::javascript
        // bad
        if (someVeryLongCondition())
          doSomething();

        for (let i = 0; i < foo.length; i++) bar(foo[i]);

        // good
        if (someVeryLongCondition()) {
          doSomething();
        }

        for (let i = 0; i < foo.length; i++) {
          bar(foo[i]);
        }

  - 다만 다음과 같이 if문에서 구문이 return 하나 뿐일 경우는 예외로 중괄호를 사용하지 않습니다.

        :::javascript
        if (shortCondition()) return;

  - **연속 블록 구문**에서 닫는 중괄호 뒤에는 개행이 아닌 스페이스 1개를 사용합니다.

        :::javascript
        // bad
        const Obj = [
          {
            // stuff..
          },
          {
            // stuff..
          }
        ];

        // good
        const Obj = [
          {
            // stuff..
          }, {
            // stuff..
          }
        ];

        // bad
        if (someFlag) {

        }
        else {

        }

        // good
        if (someFlag) { 

        } else {

        }

        // bad
        try {

        } 
        catch (e) {

        }

        // good
        try {

        } catch (e) {

        }

  - 비어있지 않는 블록은 **Kernighan and Ritchie style**을 사용합니다.
      - 여는 중괄호 앞에는 개행을 하지 않습니다
      - 여는 중괄호 뒤에는 개행을 합니다
      - 닫는 중괄호 앞에는 개행을 합니다
      - 연속 블록 구문이 아닐 경우, 닫는 중괄호 뒤에는 개행을 합니다. 즉 `else`, `catch`, `while`, 또는 comma, semicolon, or right-parenthesis 경우엔 개행을 하지 않습니다.

            :::javascript
            class InnerClass {
              constructor() {}

              method(foo) {
                if (condition(foo)) {
                  try {
                    // Note: this might fail.
                    something();
                  } catch (err) {
                    recover();
                  }
                }
              }
            }


  - 비어있는 블록은 중괄호 사이에 어떠한 공백도 넣지 않으며, 여는 블록의 앞과 뒤, 닫는 불록의 앞에는 개행을 하지 않습니다(e.g. `{}`). 다만 연속 블록 구문의 경우는 예외로 비어있지 않는 규칙을 따릅니다.

        :::javascript
        // bad
        if (condition) {
          // …
        } else if (otherCondition) {} else {
          // …
        }

        try {
          // …
        } catch (e) {}

        // good
        if (condition) {
          // …
        } else if (otherCondition) {

        } else {
          // …
        }

        try {
          // …
        } catch (e) {

        }


### 4.2 들여쓰기
  - 들여쓰기로 **스페이스 2개**를 사용합니다.
  - 저장소에 업로드 하기 전엔 선호하는 방법을 사용해도 좋습니다. **탭을 사용해도 괜찮음**을 의미합니다.
  - 다만 저장소에 업로드할 시엔 꼭 들여쓰기로 스페이스 2개를 사용하세요.

### 4.3 구문
  - 한 줄에 한개의 구문만 적습니다.
  - 구문 끝에는 세미콜론을 적습니다.

        :::javascript
        // bad
        object.doSomething(); object.doSomethingElse(); object.finish();

        // bad
        object.doSomething()
        object.doSomethingElse()
        object.finish()

        // good
        object.doSomething();
        object.doSomethingElse();
        object.finish();


### 4.4 Line-wrapping
  - **문법적 수준**에 따라 개행 및 들여쓰기를 합니다.
  - 이 때 들여쓰기는 **스페이스 4개**를 사용합니다.

        :::javascript
        // bad
        this.foo = foo(firstArg, 1 +
            someLongFunctionName());

        // good
        this.foo =
            foo(
                firstArg,
                1 + someLongFunctionName());

  - 메서드 체이닝의 경우 **작업 chunk 단위**로 점(`.`) 앞에서 개행 및 들여쓰기를 합니다.

        :::javascript
        // bad
        $('#items').find('.selected').highlight().end().find('.open').updateCount();

        // bad
        $('#items').
            find('.selected').
                highlight().
                end().
            find('.open').
                updateCount();

        // good
        $('#items')
            .find('.selected')
                .highlight()
                .end()
            .find('.open')
                .updateCount();

        // bad
        const leds =
            stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
                .attr('width', (radius + margin) * 2).append('svg:g')
                .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
                .call(tron.led);

        // good
        const leds =
            stage
                .selectAll('.led').data(data).enter()
                    .append('svg:svg')
                        .classed('led', true)
                        .attr('width', (radius + margin) * 2)
                    .append('svg:g')
                        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
                        .call(tron.led);


### 4.5 공백
  - 제어구문의 소괄호(`()`) 앞에는 스페이스 1개를 넣습니다. 단 함수선언이나 호출시 인수리스트 앞에는 넣지 않습니다.

        :::javascript
        // bad
        if(isJedi) {
          fight ();
        }

        // good
        if (isJedi) {
          fight();
        }

        // bad
        function fight () {
          console.log ('Swooosh!');
        }

        // good
        function fight() {
          console.log('Swooosh!');
        }

  - 연산자 사이에는 스페이스 1개를 넣습니다.

        :::javascript
        // bad
        const x=y+5;

        // good
        const x = y + 5;

  - 수직 정렬은 하지 않습니다

        :::javascript
        // bad
        {
          tiny:   42,
          longer: 435,
        };

        // good
        {
          tiny: 42,
          longer: 435,
        };

  - 블록 끝과 다음 구문 사이에는 개행을 한개 넣습니다.

        :::javascript
        // bad
        if (foo) {
          return bar;
        }
        return baz;

        // good
        if (foo) {
          return bar;
        }

        return baz;

        // bad
        const obj = {
          foo() {
          },
          bar() {
          },
        };
        return obj;

        // good
        const obj = {
          foo() {
          },

        bar() {
          },
        };
  
        return obj;
  
        // bad
        const arr = [
          function foo() {
          },
          function bar() {
          },
        ];
        return arr;

        // good
        const arr = [
          function foo() {
          },

          function bar() {
          },
        ];

        return arr;


### 4.6 소괄호를 이용한 그루핑
  - 구문해석기가 잘못 해석할 여지가 있는 구문 또는, 가독성을 증가시키는 경우가 아니라면 불필요한 소괄호를 사용하지 않습니다.
  - 다음의 표현식에서 소괄호를 이용하여 그루핑하지 않습니다: `delete`, `typeof`, `void`, `return`, `throw`, `case`, `in`, or `of`

        :::javascript
        // bad
        delete (variable);
        typeof (variable);

        // good
        delete variable;
        typeof variable;

### 4.7 콤마
  - 콤마로 시작하지 않습니다.

        :::javascript
        // bad
        const story = [
            once
          , upon
          , aTime
        ];

        // good
        const story = [
          once,
          upon,
          aTime,
        ];

        // bad
        const hero = {
            firstName: 'Ada'
          , lastName: 'Lovelace'
          , birthYear: 1815
          , superPower: 'computers'
        };

        // good
        const hero = {
          firstName: 'Ada',
          lastName: 'Lovelace',
          birthYear: 1815,
          superPower: 'computers',
        };

  - 끝 부분의 콤마를 사용합니다. 이는 깨끗한 git의 diff로 이어집니다.

        :::javascript
        // bad - git diff without trailing comma
        const hero = {
             firstName: 'Florence',
        -    lastName: 'Nightingale'
        +    lastName: 'Nightingale',
        +    inventorOf: ['coxcomb graph', 'modern nursing']
        };

        // good - git diff with trailing comma
        const hero = {
             firstName: 'Florence',
             lastName: 'Nightingale',
        +    inventorOf: ['coxcomb chart', 'modern nursing'],
        };

        // bad
        const hero = {
          firstName: 'Dana',
          lastName: 'Scully'
        };

        const heroes = [
          'Batman',
          'Superman'
        ];

        // good
        const hero = {
          firstName: 'Dana',
          lastName: 'Scully',
        };

        const heroes = [
          'Batman',
          'Superman',
        ];

## 5 JavaScript 코드 작성
### 5.1 변수 선언
  - **모든 변수 선언**에는 `const`와 `let`키워드를 사용합니다. 재할당이 필요한 변수가 아니라면 기본적으로 `const`를 사용합니다.
  - **하나의 변수 선언**에는 하나의 `const`키워드를 사용합니다.

        :::javascript
        // bad
        const items = getItems(),
            goSportsTeam = true,
            dragonball = 'z';

        // bad
        // (compare to above, and try to spot the mistake)
        const items = getItems(),
            goSportsTeam = true;
            dragonball = 'z';

        // good
        const items = getItems();
        const goSportsTeam = true;
        const dragonball = 'z';

  - 지역 변수는 그것이 사용되기 전에 선언 및 초기화 합니다. `const`와 `let`키워드는 함수스코프가아닌 블록스코프를 사용하기 때문입니다.

        :::javascript
        // bad
        function(hasName) {
          const name = getName();

          if (!hasName) {
            return false;
          }

          this.setFirstName(name);

          return true;
        }

        // good
        function(hasName) {
          if (!hasName) {
            return false;
          }

          const name = getName();
          this.setFirstName(name);

          return true;
        }

  - 우선 `const` 를 그룹화하고 다음에 `let`을 그룹화 합니다. 이전에 할당한 변수에 대해 나중에 새 변수를 추가하는 경우에 유용합니다.

        :::javascript
        // bad
        let i, len, dragonball,
            items = getItems(),
            goSportsTeam = true;

        // bad
        let i;
        const items = getItems();
        let dragonball;
        const goSportsTeam = true;
        let len;

        // good
        const goSportsTeam = true;
        const items = getItems();
        let dragonball;
        let i;
        let length;

### 5.2 형
  - 기본형: 기본형은 그 값을 직접 조작합니다: `string`, `number`, `Boolean`, `null`, `undefined`

        :::javascript
        const foo = 1;
        let bar = foo;

        bar = 9;

        console.log(foo, bar); // => 1, 9

  - 참조형: 참조형(Complex)은 참조를 통해 값을 조작합니다: `object`, `array`, `function`

        :::javascript
        const foo = [1, 2];
        const bar = foo;

        bar[0] = 9;

        console.log(foo[0], bar[0]); // => 9, 9

### 5.3 Array
  - 배열 작성에는 **리터럴 구문**을 사용합니다. 명시적 공간 할당을 위한 `new Array(length)`사용은 허용됩니다.

        :::javascript
        // bad
        const items = new Array();

        // good
        const items = [];

        // it's ok
        const stocks = new Array(10);

  - 배열 인덱스로 **Non-Numeric 타입**을 사용하지 않습니다. 대신 Map, Object를 이용할 수 있습니다.

        :::javascript
        const someStack = [];

        // bad
        someStack['abra'] = 'abracadabra';

        // it's ok, but isn't recommended
        someStack[0] = 'abracadabra';

        // good
        someStack.push('abracadabra');

  - 배열의 복사, 배열 합치기에는 배열의 확장연산자 `...` 를 이용합니다.

        :::javascript
        // bad
        const itemsCopy = Array.prototype.slice.call(items);

        // good
        const itemsCopy = [...items];

        // bad
        const allItems = itemsA.concat(itemsB);

        // good
        const allItems = [...itemsA, ...itemsB];

  - array-like 오브젝트를 배열로 변환하는 경우는 Array#from을 이용할 수 있습니다.

        :::javascript
        const foo = document.querySelectorAll('.foo');
        const nodes = Array.from(foo);

### 5.4 Object
  - 오브젝트 작성에는 **리터럴 구문**을 사용합니다.

        :::javascript
        // bad
        const item = new Object();

        // good
        const item = {};

  - **구조체 스타일**과 **사전 스타일**의 key 정의를 혼용하지 않습니다.

        :::javascript
        // bad
        {
          a: 42, // struct-style unquoted key
          'b': 43, // dict-style quoted key
        }

        // good
        {
          a: 42,
          b: 43,
        }

        // Or...
        {
          'a': 42, 
          'b': 43, 
        }

  - 동적 프로퍼티명을 갖는 오브젝트를 작성할때, 계산된 프로퍼티명(computed property names)을 이용합니다. 이 때 계산된 프로퍼티명은 사전 스타일로 취급됩니다.

        :::javascript
        function getKey(k) {
          return a `key named ${k}`;
        }

        // bad
        const obj = {
          id: 5,
          name: 'San Francisco',
        };
        obj[getKey('enabled')] = true;

        // good
        const obj = {
          'id': 5,
          'name': 'San Francisco',
          getKey('enabled')]: true,
        };

  - 메서드 단축구문을 이용합니다.

        :::javascript
        // bad
        const atom = {
          value: 1,

          addValue: function (value) {
            return atom.value + value;
          },
        };

        // good
        const atom = {
          value: 1,

          addValue(value) {
            return atom.value + value;
          },
        };

  - 프로퍼티 단축구문을 이용합니다.

        :::javascript
        const lukeSkywalker = 'Luke Skywalker';

        // bad
        const obj = {
          lukeSkywalker: lukeSkywalker,
        };

        // good
        const obj = {
          lukeSkywalker,
        };

  - 프로퍼티의 단축구문은 오브젝트 선언의 시작부분에 그룹화 합니다.

        :::javascript
        const anakinSkywalker = 'Anakin Skywalker';
        const lukeSkywalker = 'Luke Skywalker';

        // bad
        const obj = {
          episodeOne: 1,
          twoJediWalkIntoACantina: 2,
          lukeSkywalker,
          episodeThree: 3,
          mayTheFourth: 4,
          anakinSkywalker,
        };

        // good
        const obj = {
          lukeSkywalker,
          anakinSkywalker,
          episodeOne: 1,
          twoJediWalkIntoACantina: 2,
          episodeThree: 3,
          mayTheFourth: 4,
        };

### 5.5 Class
  - 생성자는 클래스 리터럴에서 첫 번째 함수정의로 등장하여야 하며, 서브클래스에서 상위클래스의 필드에 접근을 할 때 항상 먼저 `super()`를 사용합니다.

        :::javascript
        class Vehicle {
          constructor(color) {
            this._color = color;
          }
        }

        class Car extends Vehicle {
          constructor(color) {
            super(color);
            this._colorCopy = this._color;
          }
        }

  - `prototype`을 통해 필드에 접근하지 않습니다.

        :::javascript
        // bad
        function Queue(contents = []) {
          this._queue = [...contents];
        }
        Queue.prototype.pop = function() {
          const value = this._queue[0];
          this._queue.splice(0, 1);
          return value;
        }

        // good
        class Queue {
          constructor(contents = []) {
            this._queue = [...contents];
          }
          pop() {
            const value = this._queue[0];
            this._queue.splice(0, 1);
            return value;
          }
        }

  - toString()을 오버라이딩은 허용하지만, 올바르게 동작하는지와 side effect 가 없는지 확인합니다.

        :::javascript
        class Jedi {
          constructor(options = {}) {
            this.name = options.name || 'no name';
          }

          getName() {
            return this.name;
          }

          toString() {
            return `Jedi - ${this.getName()}`;
          }
        }

### 5.6 Function
  - 절대 새 함수를 작성하기 위해 Function constructor를 이용하지 않습니다. `eval()` 과 같은 수준의 취약점을 일으킬 수 있습니다

        :::javascript
        // bad
        var add = new Function('a', 'b', 'return a + b');

        // still bad
        var subtract = Function('a', 'b', 'return a - b');

  - 함수식을 이용하는 경우 **화살표 함수**를 이용합니다. `f.bind(this)` 또는 `const self = this`의 사용을 피합니다.

        :::javascript
        // bad
        [1, 2, 3].map(function (x) {
          const y = x + 1;
          return x * y;
        });

        // good
        [1, 2, 3].map((x) => {
          const y = x + 1;
          return x * y;
        });

  - 함수의 본체가 하나의 식으로 구성된 경우에는 중괄호(`{}`)를 생략하고 암시적 return을 이용하는 것이 가능합니다. 그 외에는 `return` 문을 이용합니다.

        :::javascript
        // good
        [1, 2, 3].map(number => `A string containing the ${number}.`);

        // bad
        [1, 2, 3].map(number => {
          const nextNumber = number + 1;
          `A string containing the ${nextNumber}.`;
        });

        // good
        [1, 2, 3].map(number => {
          const nextNumber = number + 1;
          return `A string containing the ${nextNumber}.`;
        });

  - 식이 복수행에 걸쳐있을 경우는 가독성을 더욱 좋게하기 위해 소괄호로 감쌉니다.

        :::javascript
        // bad
        [1, 2, 3].map(number => 'As time went by, the string containing the ' +
          `${number} became much longer. So we needed to break it over multiple ` +
          'lines.'
        );

        // good
        [1, 2, 3].map(number => (
          `As time went by, the string containing the ${number} became much ` +
          'longer. So we needed to break it over multiple lines.'
        ));

  - **함수이외의 블록 (if나 while, 특히 루프문)** 안에서 함수를 선언하지 마십시오. 변수에 함수를 대입하는 대신 브라우저들은 그것을 허용하지만 모두가 다르게 해석합니다.

  - ECMA-262 사양에서는 block 은 statements의 일람으로 정의되어 있지만 **함수선언은 statements가 아닙니다.** 따라서 함수 정의문에서는 세미콜론(`;`)을 붙이지 않습니다.

        :::javascript
        // bad
        if (currentUser) {
          function test() {
            console.log('Nope.');
          }
        }

        // good
        let test;
        if (currentUser) {
          test = () => {
            console.log('Yup.');
          };
        }

  - 절대 파라메터로 `arguments`를 사용하지 않습니다. 이것은 함수 스코프에 전해지는 arguments 오브젝트의 참조를 덮어써 버립니다. 대신에 **확장 배열 연산자**를 이용합니다.

        :::javascript
        // bad
        function nope(name, options, arguments) {
          // ...stuff...
        }

        // bad
        function concatenateAll(name, options) {
          const args = Array.prototype.slice.call(arguments, 2);
          return args.join('');
        }

        // good
        function concatenateAll(name, options, ...args) {
          return args.join('');
        }

  - 함수의 파라메터를 수정하는 것보다 default 파라메터를 이용합니다.

        :::javascript
        // really bad
        function handleThings(opts) {
          // 만약 opts가 falsy 하다면 바라는대로 비어있는 오브젝트가 설정됩니다.
          // 하지만 미묘한 버그를 일으킬지도 모릅니다.
          opts = opts || {};
          // ...
        }

        // still bad
        function handleThings(opts) {
          if (opts === void 0) {
            opts = {};
          }
          // ...
        }

        // good
        function handleThings(opts = {}) {
          // ...
        }

  - side effect가 있을 default 파라메터의 이용은 피합니다. 코드 해석에 혼동을 야기할 여지가 있습니다.
    
        :::javascript
        var b = 1;
        // bad
        function count(a = b++) {
          console.log(a);
        }
        count();  // 1
        count();  // 2
        count(3); // 3
        count();  // 3

  - 항상 default 파라메터는 뒤쪽에 두십시오.

        :::javascript
        // bad
        function handleThings(opts = {}, name) {
          // ...
        }

        // good
        function handleThings(name, opts = {}) {
          // ...
        }

### 5.7 String
  - 문자열에는 더블쿼트 `""` 대신에 싱크쿼트 `''` 를 사용합니다. 문자열에 싱글쿼트가 들어간다면 **문자열 템플릿**을 사용하면 좋습니다.

        :::javascript
        // bad
        const name = "MR. Charles didn't come";

        // good
        const name = 'MR. Charles didn\'t come';
        const name = `MR. Charles didn't come`;

  - **60문자 이상의 문자열**은 문자열연결을 사용해서 복수행에 걸쳐 기술할 필요가 있습니다. 이 때 문자열 연속 기호는 사용하지 않습니다.

        :::javascript
        // bad
        const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

        // bad
        const errorMessage = 'This is a super long error that was thrown because \
        of Batman. When you stop to think about how Batman had anything to do \
        with this, you would get nowhere \
        fast.';

        // good
        const errorMessage = 'This is a super long error that was thrown because ' +
            'of Batman. When you stop to think about how Batman had anything to do ' +
            'with this, you would get nowhere fast.';

  - 프로그램에서 문자열을 생성하는 경우는 문자열 연결이 아닌 문자열 템플릿을 이용해 주십시오.

        :::javascript
        // bad
        function sayHi(name) {
          return 'How are you, ' + name + '?';
        }

        // bad
        function sayHi(name) {
          return ['How are you, ', name, '?'].join();
        }

        // good
        function sayHi(name) {
          return `How are you, ${name}?`;
        }

### 5.8 형변환
  - 필요한 경우 문의 선두에서 형변환을 시도합니다.
  - 문자열의 경우

        :::javascript
        // bad
        const totalScore = this.reviewScore + '';

        // good
        const totalScore = String(this.reviewScore);

  - 수치의 경우, 항상 `parseInt()` 또는 `parseFloat()`을 사용하고 형변환을 위한 기수를 전달합니다.

        :::javascript
        const inputValue = '4';

        // bad
        const val = new Number(inputValue);

        // bad
        const val = +inputValue;

        // bad
        const val = inputValue >> 0;

        // bad
        const val = parseInt(inputValue);

        // good
        const val = Number(inputValue);

        // good
        const val = parseInt(inputValue, 10);

  - 무언가의 이유로 인해 parseInt 가 bottleneck 이 되어 성능적인 이유로 Bitshift를 사용할 필요가 있는 경우 하려고 하는 경우, 그 이유와 무엇을 shift하는지 코멘트로 달아놓습니다.

        :::javascript
        // good
        /**
         * parseInt 가 원인으로 느렸음.
         * Bitshift를 통한 수치로의 문자열 강제 형변환으로
         * 성능을 개선시킴.
         */
        const val = inputValue >> 0;

  - 부울의 경우

        :::javascript
        const age = 0;

        // bad
        const hasAge = new Boolean(age);

        // good
        const hasAge = Boolean(age);

        // good
        const hasAge = !!age;

### 5.9 제어 구문
#### 5.9.1 For loops
  - 가능한 한 `for-of` 루프 대신에 `map()` 과 `reduce()` 와 같은 JavaScript **고급함수(higher-order functions)**를 이용합니다. 고급함수는 immutable(불변)룰을 적용하므로 side effect에 대해 추측하는 것보다 값을 반환하는 순수 함수를 다루는게 간단합니다.

        :::javascript
        const numbers = [1, 2, 3, 4, 5];

        // bad
        let sum = 0;
        for (let num of numbers) {
          sum += num;
        }

        sum === 15;

        // good
        let sum = 0;
        numbers.forEach((num) => sum += num);
        sum === 15;

        // best (use the functional force)
        const sum = numbers.reduce((total, num) => total + num, 0);
        sum === 15;

#### 5.9.2 Error Handling
  - 항상 `Error` 또는 그 서브 클래스를 반환해야 합니다. 절대로 스트링 리터럴이나 `Error` 외의 다른 객체를 `throw`하지 마십시오.

        :::javascript
        // bad
        if (err) {
          throw `error! ${err}`;
        }

        // good
        if (err) {
          throw new Error(`error! ${err}`);
        }

#### 5.9.3 비교 연산
  - `==` 이나 `!=` 대신에 `===` 와 `!==` 을 사용합니다.        
  - 단축형을 사용합니다.

        :::javascript
        // bad
        if (name !== '') {
          // ...stuff...
        }

        // good
        if (name) {
          // ...stuff...
        }

        // bad
        if (collection.length > 0) {
          // ...stuff...
        }

        // good
        if (collection.length) {
          // ...stuff...
        }

#### 5.9.4 Switch
  - Switch 블럭에서 어느 구문에서든 Switch 블럭을 탈출하지 않고 다음 구문으로 넘어갈 수 있습니다. 이 때 어떤 식으로든 **다른사람이 이 사실을 놓치지 않게 해야 합니다.** 코멘트를 사용합니다.

        :::javascript
        switch (input) {
          case 1:
          case 2:
            prepareOneOrTwo();
            // fall through
          case 3:
            handleOneTwoOrThree();
            break;
          default:
            handleLargeNumber(input);
        }

### 5.10 코멘트
  - 단일행 코멘트에는 `//` 을 사용해 주십시오. 코멘트를 추가하고 싶은 코드의 상부에 배치해 주십시오. 또한, 코멘트의 앞에 빈행을 넣습니다.

        :::javascript
        // bad
        const active = true;  // is current tab

        // good
        // is current tab
        const active = true;

        // bad
        function getType() {
          console.log('fetching type...');
          // set the default type to 'no type'
          const type = this._type || 'no type';

          return type;
        }

        // good
        function getType() {
          console.log('fetching type...');

          // set the default type to 'no type'
          const type = this._type || 'no type';

          return type;
        }

        // also good
        function getType() {
          // set the default type to 'no type'
          const type = this._type || 'no type';

          return type;
        }

  - 복수행의 코멘트는 `/** ... */` 을 사용해 주십시오.
  - 모든 클래스, 메서드, 필드에는 [JSDoc 3 형식](http://usejsdoc.org/)의 설명을 답니다.

        :::javascript
        // bad
        // make() returns a new element
        // based on the passed in tag name
        //
        // @param {String} tag
        // @return {Element} element
        function make(tag) {

          // ...stuff...

          return element;
        }

        // good
        /**
         * make() returns a new element
         * based on the passed in tag name
         *
         * @param {String} tag
         * @return {Element} element
         */
        function make(tag) {

          // ...stuff...

          return element;
        }

  - 문제에 대한 주석으로써 `// FIXME:` 를 사용합니다.

        :::javascript
        class Calculator extends Abacus {
          constructor() {
            super();

            // FIXME: 글로벌변수를 사용해서는 안됨.
            total = 0;
          }
        }

  - 문제의 해결책에 대한 주석으로 `// TODO:` 를 사용합니다.

        :::javascript
        class Calculator extends Abacus {
          constructor() {
            super();

            // TODO: total 은 옵션 파라메터로 설정해야함.
            this.total = 0;
          }
        }

### 5.11 Destructuring
  - 배열의 Destructuring을 이용합니다.

        :::javascript
        const arr = [1, 2, 3, 4];

        // bad
        const first = arr[0];
        const second = arr[1];

        // good
        const [first, second] = arr;

  - 복수의 값을 반환하는 경우는 배열의 Destructuring이 아닌 오브젝트의 Destructuring을 이용합니다.

        :::javascript
        // bad
        function processInput(input) {
          return [left, right, top, bottom];
        }

        // 호출처에서 반환된 데이터의 순서를 고려해야 합니다.
        const [left, __, top] = processInput(input);

        // good
        function processInput(input) {
          return { left, right, top, bottom };
        }

        // 호출처에서는 필요한 데이터만 선택하면 됩니다.
        const { left, right } = processInput(input);

### 5.12 사용하면 안되는 것들
#### 5.12.1 with
  - with 키워드는 코드를 이해하기 어렵게 만듭니다.

#### 5.12.2 eval
  - **"eval is evil"** - Douglas Crockford

#### 5.12.3 Function
  - Function 키워드를 이용한 동적 해석은 eval과 마찬가지로 코드에 많은 취약점과 성능 저하를 야기합니다.

#### 5.12.4 기본형의 래퍼 클래스에 대한 new 키워드 사용
  - 기본형에 대한 new 키워드 사용은 예상과 다른 결과를 반환합니다.

         :::javascript
         // bad
         const x = new Boolean(false);
         if (x) alert(typeof x);  // alerts 'object'

         const x = Boolean(0);
         if (!x) alert(typeof x);  // alerts 'boolean', as expected