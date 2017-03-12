# WRL JavaScript 스타일 가이드

## 원문
 - [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
 - [Airbnb JavaScript Style Guide(en)](https://github.com/airbnb/javascript)
 - [Airbnb JavaScript Style Guide(ko)](https://github.com/tipjs/javascript-style-guide)

## 목차

  1. [개요](#_1)
  1. [소스 파일]()
      1. [파일 이름]()
      1. [파일 인코딩]()
      1. [특수 문자]()
  1. [네이밍]()
      1. [기본 규칙]()
      1. [식별자 타입에 따른 규칙]()
          1. [Object]()
          1. [Class]()
          1. [Property]()
          1. [Constant]()
          1. [Parameter]()
  1. [JavaScript 코드 작성]()
    1. [중괄호 표기]()
    1. [블록 들여쓰기]()

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
        }

        // 파일 이름
        makeStyleGuide.js

  - 파일을 1개의 클래스로 export 하는 경우, PascalCase 명명규칙을 따릅니다. 파일명은 클래스명과 일치시킵니다.

        :::javascript
        // 파일 내용
        class CheckBox {
          // ...
        }
        export default CheckBox;

        // HTML에서 로드할 때
        // bad
        <script src="./checkBox"></script>

        // bad
        <script src="./check_box"></script>

        // good
        <script src="./CheckBox"></script>

  - singleton / function library / 빈오브젝트를 export 하는 경우, PascalCase 명명규칙을 따릅니다.

        :::javascript
        const AirbnbStyleGuide = {
          es6: {
          }
        };

        // 파일 이름
        AirbnbStyleGuide.js;


### 2.2 파일 인코딩
  - 소스 파일의 문자 인코딩 방식은 **UTF-8**을 사용합니다.

### 2.3 특수 문자
  - **공백 문자**는 오직 ASCII horizontal space character (0x20) 만을 사용합니다. 이는 **들여쓰기로 탭을 사용하지 않음**을 의미합니다.
  - ASCII로 표현가능한 **이스케이프 문자** (`\'`, `\"`, `\\`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`) 는 숫자 기반의 이스케이프 표현 (e.g `\x0a`, `\u000a`, or `\u{a}`) 을 사용하지 않습니다.
  - **Non-ASCII 문자**는 **유니코드 이스케이프**를 사용합니다. 이 때 주석으로 어떤 문자인지 설명을 달아줍니다.
| Example  | Discussion  |
|---|---|
| const units = 'μs';  | Best, 주석이 필요 없이 명확함  |
| const units = '\u03bcs'; // 'μs'   | Allowed  |
| const units = '\u03bcs'; // Greek letter mu, 's'   | Allowed  |
| const units = '\u03bcs';  | Poor, 무슨 문자인지 알 수 없음  || return '\ufeff' + content; // byte order mark  | Good, 출력 불가능한 문자 표현을 위해 이스케이프하였으며, 주석이 적절함  |

## 3 네이밍
### 3.1 기본 규칙
  - 모든 식별자는 ASCII 문자와 숫자, 밑줄, 달러 기호를 이용하여 만듭니다.
  - 축약어는 흔히 알려진 것만을 사용할 수 있으며, 식별자의 의미를 쉽게 파악할 수 있게 descriptive한 이름을 사용합니다.

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
  - Class, interface, record, typedef name은 PascalCase 명명규칙을 따릅니다.

        :::javascript
        // bad
        function user(options) {
          this.name = options.name;
        }

        const bad = new user({
          name: 'nope',
        });

        // good
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


## 4 JavaScript 코드 작성
### 4.1 중괄호
  - 모든 제어구문(e.g. `if`, `else`, `for`, `do`, `while`, 등)에서 중괄호를 반드시 사용합니다.
  - 중괄호 앞에는 스페이스 1개를 넣습니다.

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

  - 연속 블록 구문에서 닫는 중괄호 뒤에는 개행이 아닌 스페이스 1개를 사용합니다.
  - 비어있지 않는 블록은 Kernighan and Ritchie style을 사용합니다.
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


  - 비어있는 블록은 중괄호 사이에 어떠한 공백도 넣지 않으며(e.g. `{}`), 여는 블록의 앞과 뒤, 닫는 불록의 앞에는 개행을 하지 않습니다.
  - 다만 연속 블록 구문의 경우는 예외로 비어있지 않는 규칙을 따릅니다.

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
  - 들여쓰기로 스페이스 2개를 사용합니다.
  - 저장소에 업로드 하기 전엔 선호하는 방법을 사용해도 좋습니다. 탭을 사용해도 괜찮음을 의미합니다.
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
  - 문법적 수준에 따라 개행 및 들여쓰기를 합니다.
  - 이 때 들여쓰기는 스페이스 4개를 사용합니다.

        :::javascript
        // bad
        this.foo = foo(firstArg, 1 +
            someLongFunctionName());

        // good
        this.foo =
            foo(
                firstArg,
                1 + someLongFunctionName());

  - 메서드 체이닝의 경우 작업 chunk 단위로 점(`.`) 앞에서 개행 및 들여쓰기를 합니다.

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


        :::javascript
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



## 형(Types)