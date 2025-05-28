# 렉시컬 스코프
> 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프(정적 스코프)라 한다.
```javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```
* 함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않는다.
* "함수의 상위 스코프를 결정한다" == "렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다 (상위 스코프)"
# 함수 객체의 내부 슬롯 [[Environment]]
> 함수는 자신의 내부 슬롯 [[Environment]] 에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.
* 함수 객체의 내부 슬롯 [[Environment]] 에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다.
* 또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값이다.
* 함수 객체는 내부 슬롯 [[Environment]] 에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.
```javascript
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
  // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
  bar();
}

// 전역에서 정의된 함수
function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```
# 클로저와 렉시컬 환경
```javascript
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () { console.log(x); }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```
* 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다.
* 이러한 중첩 함수를 클로저(closure) 라고 부른다.   

위 예제에서,   
① outer 함수가 평가되어 함수 객체를 생성할 때 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수 객체의 [[Environment]] 내부 슬롯에 상위 스코프로서 저장   
② 런타임에 중첩 함수 inner를 평가, 자신의 [[Environment]] 내부 슬롯에 outer 함수의 렉시컬 환경을 상위 스코프로서 저장   
③ outer 함수의 실행이 종료, inner 함수를 반환하면서 outer 함수의 생명 주기 종료(실행 컨텍스트 스택에서 제거) but outer 함수의 렉시컬 환경까지 소멸하는 것은 X   
  outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상 X   
④ outer 함수가 반환한 inner 함수를 호출, inner 함수의 실행 컨텍스트 생성, 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 참조값 할당   
중첩 함수 inner의 내부에서는 상위 스코프를 참조할 수 있으므로 상위 스코프의 식별자를 참조할 수 있고 식별자의 값을 변경할 수도 있다.   
* 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.
* 이때 클로저가 참조하고 있지 않는 식별자는 기억하지 않는다.
# 클로저의 활용
> 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.
```javascript
// 카운트 상태 변경 함수
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저인 메서드를 갖는 객체를 반환한다.
  // 객체 리터럴은 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
  return {
    // num: 0, // 프로퍼티는 public하므로 은닉되지 않는다.
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    }
  };
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```
* 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉(information hiding)하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.
# 캡슐화와 정보 은닉
> 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.
```javascript
const Person(function () {
  let _age = age;   // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```
* 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉(information hiding) 이라 한다.
* 자바스크립트는 접근 제한자를 제공하지 않지만 클로저를 사용하면 정보 은닉이 가능한 것처럼 보인다. but 정보 은닉을 완전하게 지원하지는 않는다.
# 자주 발생하는 실수
> 클로저를 사용할 때 자주 발생하는 실수이다.
```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```
* 위 예제에서, 배열 funcs의 요소로 추가된 3개의 함수가 0, 1, 2를 반환할 것으로 기대하지만 결과는 3, 3, 3을 출력한다.
* 이는 var 키워드로 선언한 변수가 함수 레벨 스코프를 갖기 때문이다. (전역 변수)
해결 방법은 다음과 같다.
1. let 키워드 사용
```javascript
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () { return i; };
}

for (let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}
```
* for 문의 변수 선언문에서 let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 반복 실행될 때마다 for 문 코드 블록의 새로운 렉시컬 환경이 생성된다.
* for 문이 반복될 때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지한다.
2. 고차 함수 사용
```javascript
const funcs = Array.from(new Array(3), (_, i) => () => i);

funcs.forEach(f => console.log(f())); // 0 1 2
```
