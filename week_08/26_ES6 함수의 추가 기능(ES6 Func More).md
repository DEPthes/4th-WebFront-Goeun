# 함수의 구분
> ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다. -> ES6 이전의 모든 함수는 callable이면서 constructor
```
📌 callable과 constructor
+ 호출할 수 있는 함수 객체 -> callable
+ 인스턴스를 생성할 수 있는 함수 객체 -> constructor
+ 인스턴스를 생성할 수 없는 함수 객체 -> non-constructor
```
* ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다. -> 실수 유발 가능성, 성능 저하
* 이러한 문제 해결을 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 구분

| ES6 함수의 구분 | constructor | prototype | super | arguments |
|---------------|-------------|-----------|-------|-----------|
| 일반 함수     | O           | O         | X     | O         |
| 메서드        | X           | X         | O     | O         |
| 화살표 함수   | X           | X         | X     | X         |
* 일반 함수는 함수 선언문 or 함수 표현식으로 정의된 함수 (ES6이전과 차이 X)
* 일반 함수는 constructor but 메서드와 화살표 함수는 non-constructor
# 메서드
> ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.
```javascript
const obj = {
  x: 1,
  // foo는 ES6 기준, 메서드다.
  foo() {
    return this.x;
  },
  // bar는 ES6 기준, 메서드가 아니다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```
* ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor이기 때문에 생성자 함수로서 호출 불가능
  * ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
* 표준 빌드인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor
* ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.
  * super 참조는 내부 슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로, ES6 메서드는 super 키워드를 사용할 수 있다.
# 화살표 함수
> 화살표 함수는 function 키워드 대신 화살표(=>)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.   
> 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.
## 화살표 함수 정의
### 함수 정의
* 화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다.
```javascript
const multiply = (x, y) => x * y;
multiply(2, 3); // 6
```
### 매개변수 선언
* 매개변수가 여러 개 -> 소괄호 안에 매개변수를 선언
* 매개변수가 한 개 -> 소괄호 생략 가능
* 매개변수 없음 -> 소괄호 생략 불가능
```javascript
// 매개변수 여러 개
const arrow = (x, y) => {...};
// 매개변수 한 개
const arrow = x => {...};
// 매개변수 없음
const arrow = () => {...};
```
### 함수 몸체 정의
* 함수 몸체가 하나의 문으로 구성
  * 몸체를 감싸는 중괄호 생략 가능
  * 값으로 평가될 수 있는 표현식인 문 -> 암묵적으로 반환
* 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸주어야 한다.
* 함수 몸체가 여러 개의 문으로 구성
  * 몸체를 감싸는 중괄호 생략 불가능
  * 반환값이 있다면 명시적으로 반환해야 한다.
* 화살표 함수도 즉시 실행 함수로 사용 가능
* 화살표 함수도 일급 객체이므로, 고차 함수(함수를 인자로 받을 수 있는 함수)에 인수로 전달 가능
  * 이 경우에는 일반적인 함수 표현식보다 표현이 간결하고 가독성이 좋다.
```javascript
// 함수 몸체가 한 줄이면, 중괄호 생략 가능
const increase = (x) => ++x;

// 객체 리터럴 반환 시, 소괄호 명시
const create = (id, content) => ({ id, content });
// == const create = (id, content) => { return { id, content }; };

// 함수 몸체가 여러 줄이면, 중괄호 명시
const sum = (a, b) => {
  const result = a + b;
  return result;
};

// 화살표 함수 즉시 실행 함수로 사용 가능
const person = ((name) => ({
  sayHi() {
    return `My name is ${name}.`;
  },
}))("Choi");

console.log(person.sayHi()); // My name is Choi.

// 화살표 함수는 일급 객체로 사용 가능 -> 고참함수의 콜백함수로 사용하기 적절
console.log([1, 2, 3].map((v) => v * 2)); // [ 2, 4, 6 ]
```
## 화살표 함수와 일반 함수의 차이
1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다.
```javascript
// 화살표 함수는 인스턴스 생성이 불가
const Foo = () => {};
new Foo(); // TypeError: Foo is not a constructor
```
2. 중복된 매개변수 이름을 선언할 수 없다.
```
// 화살표 함수는 매개변수 중복을 허용하지 않음
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```
3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
   * 따라서, 스코프 체인을 통해 상위 스코프의 this, arguments super, new.target 을 참조
   * 화살표 함수가 중첩되어 있어도, 화살표 함수들은 자체적으로 바인딩하지 않으므로 외부에 가장 가까운 상위 스코프 함수의 것을 참조
## this
> 일반적으로 this 바인딩은 함수의 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.
* 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
  * 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. -> lexical this
  * 화살표 함수의 this가 삼수가 정의된 위치에 의해 결정된다는 것을 의미한다.
```javascript
// 💡 화살표 함수는 this, arguments, super, new.target 바인딩을 갖지 않는다.
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  // 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
  // 화살표 함수 내부에서 this를 참조하면 상위 스코프(현 시점에는 Prefixer 객체)의 this를 그대로 참조
  // = 렉시컬 this
  add(arr) {
    return arr.map((item) => this.prefix + item);
  }
}

const prefixer = new Prefixer("pre-");
console.log(prefixer.add(["AAA", "BBB"])); // [ 'pre-AAA', 'pre-BBB' ]
```
* 메서드를 정의할 때 화살표 함수를 사용하는 것은 좋지 않다. ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.
## super
> 화살표 함수는 함수 자체의 super 바인딩을 갖지 않으므로 상위 스코프의 super를 참조한다.
```javascript
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `My name is ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super
  // 왜냐하면 Derived의 sayHi는 화살표 함수고, super 바인딩을 갖지 않으므로, constructor의 super를 참조하는 것
  // constructor는 생략되었지만, 암묵적으로 constructor가 생성된다.
  sayHi = () => `${super.sayHi()}`;
}

const derived = new Derived("Choi");
console.log(derived.sayHi()); // My name is Choi
```
## arguments
> 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않으므로 상위 스코프의 arguments를 참조한다.
* arguments 객체는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용한데, 화살표 함수는 상위 함수에게 전달된 인수 목록을 참조하므로 도움이 되지 않는다.
  * 따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 Rest 파라미터를 사용해야 한다.
```javasciprt
// 화살표 함수는 자체적으로 arguments 객체를 생성하지 않는다.
// 전역에는 arguments 객체가 존재하지 않는다. (함수 내부에서만 유효)
const foo = () => console.log(arguments);

foo(1, 2); // ReferenceError: arguments is not defined
```
# Rest 파라미터
## 기본 문법
> Rest 파라미터는 매개변수 이름 앞에 세개의 점 (...)을 붙여서 정의한 매개변수를 의미한다.
* Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
* 일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다.
  * 함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당된다.
  * Rest 파라미터는 반드시 마지막 파라미터이어야 한다.
* Rest 파라미터는 단 하나만 선언할 수 있다.
* Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.
```javascript
// Rest 파라미터
function foo(a, b, ...rest) {
  console.log(a, b, rest, `함수 객체의 length 프로퍼티 : ${foo.length}`);
}

foo(1, 2, 3, 4, 5); // 1 2 [ 3, 4, 5 ] 함수 객체의 length 프로퍼티 : 2
```
## Rest 파라미터와 arguments 객체
> ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다.
* 유사 배열 객체인 arguments 객체를 배열로 반환하는 번거로움을 피할 수 있다.
# 매개변수 기본값
> 자바스크립트 엔진은 매개변수의 개수와 인수의 개수를 체크하지 않는다.
* 인수가 전달되지 않은 매개변수의 값은 undefined다.
  * 따라서 매개변수에 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할 필요가 있다. (방어코드 필요)
* ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.
  * 매개변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.
  * Rest 파라미터에는 기본값을 지정할 수 없다.
  * 매개변수 기본값은 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.
```javascript
// ES6 매개변수 초기화
function sum(a = 0, b = 0) {
  return a + b;
}

console.log(sum()); // 0
console.log(sum(1)); // 1
console.log(sum(1, 2)); // 3
```
