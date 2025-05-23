# 함수
> 프로그래밍 언어의 함수는 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것
* 함수 내부로 입력을 전달받는 변수를 배개변수, 입력을 인수, 출력을 반환값이라 한다.
* 함수는 함수 정의를 통해 생성한다.
* 인수를 매개변수를 통해 함수에 전달하면서 함수의 실행을 명시적으로 지시해야하는데, 이를 함수 호출이라 한다.
# 함수를 사용하는 이유
1. 함수는 필요할 때 여러 번 호출 가능 -> 코드의 재사용이라는 측면에서 유리
2. 코드의 중복을 억제, 재사용성을 높임 -> 유지보수의 편의성과 코드의 신뢰성 향상
3. 함수는 객체 타입의 값이므로 이름을 붙일 수 있는데, 적절한 함수 이름은 함수의 역할 파악 가능 -> 코드의 가독성 향상
# 함수 리터럴
> 자바스크립트의 함수는 객체 타입의 값이므로 함수를 함수 리터럴로 생성 가능
* 함수 리터럴은 function 키워드, 함수 이름, 매개변수 목록, 함수 몸체로 구성된다.
```javascript
// 변수에 "함수 리터럴"을 할당
var f = function add(x, y) {
  return x + y;
};
```
* 리터럴은 값을 생성하기 위한 표기법이기에 함수 리터럴도 평가되어 값을 생성하며, 이 값은 객체다.
* 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
# 함수 정의
> 함수 정의란 함수를 호출하기 이전에 인수를 전달받을 매개변수와 실행할 문들, 반환할 값을 지정하는 것을 말한다.   
> 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 된다.
* 함수 정의 방식 4가지
1. 함수 선언문
```javascript
function add(x, y) {
return x+y;
}
```
2. 함수 표현식
```javascript
var add = function(x, y) {
return x+y;
};
```
3. Function 생성자 함수
```javascript
var add = new Function('x', 'y', 'return x+y');
```
4. 화살표 함수(ES6)
```javascript
var add = (x, y) => x+y;
```
## 함수 선언문
> 함수 선언문은 함수 리터럴과 형태가 동일하다. 단, 함수 선언문은 함수 이름을 생략할 수 없다.
* 함수 선언문은 표현식이 아닌 문이다. 하지만 함수 선언문이 변수에 할당되는 것처럼 보이는 이유?
### 코드의 문맥에 따른 자바스크립트 엔진의 함수 해석
> 자바스크립트 엔진은 코드의 문맥에 따라 동일한 함수 리터럴을 함수 표현식 또는 함수 선언문으로 해석하는 경우가 있다.
* {}은 코드 블록일 수도 있고, 객체 리터럴일 수도 있다.
  * {}이 단독으로 존재 -> 자바스크립트 엔진은 {}을 블록문으로 해석
  * {}이 값으로 평가되어야 할 문맥에서 피연산자로 사용될 경우 -> 자바스크립트 엔진은 {}을 객체 리터럴로 해석
* 함수 리터럴이 단독으로 사용된다 -> 함수 선언문으로 해석
* 함수 리터럴이 값으로 평가되어야 하는 문맥 -> 함수 리터럴 표현식으로 해석
## 함수 선언문과 함수 리터럴 표현식
```javascript
// 1️⃣ 함수 리터럴 표현식으로 함수 호출
(function bar() {
  console.log("bar"); // ReferenceError: bar is not defined
});
bar();

// 2️⃣ 함수 선언문으로 함수 호출
function foo() {
  console.log("foo"); // foo
}
foo();
```
* 함수 몸체 외부에서는 함수 이름으로 함수를 호출할 수 없다.
* 함수 선언문으로 함수 호출 시 호출이 가능한 이유?
  * 자바스크립트 엔진은 함수 선언문을 해석해 함수 객체를 생성
  * 생성된 함수 객체를 가리키는 유효한 식별자가 필요
  * 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성, 거기에 함수 객체를 할당
* 결국, 함수는 함수 이름으로 호출하는 것이 아니라, 함수 객체를 가리키는 식별자로 호출하는 것이다.
## 함수 표현식
> 값의 성질을 갖는 객체를 일급 객체라 하는데, 자바스크립트의 함수는 일급 객체이기 때문에 함수를 값처럼 자유롭게 사용할 수 있다.
* 함수 리터럴로 생성한 함수 객체를 변수에 할당한 것을 함수 표현식이라고 한다.
## 함수 생성 시점과 함수 호이스팅
> 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르다.
* 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있다.
  * 자바스크립트 엔진은 런타임 이전에 함수 객체를 먼저 생성하고, 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 생성된 함수 객체를 할당한다.
  * 함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징 -> **함수 호이스팅**
  * 함수 선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화된다.
* 함수 표현식은 변수에 할당되는 값이 함수 리터럴인 문이다.
  * 변수 선언문과 변수 할당문을 한 번에 기술한 축약 표현과 동일하게 동작
  * 변수 선언은 런타임 이전에 실행되어 undefined로 초기화, 변수 할당문의 값은 런타임에 평가되므로 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.
  * 함수 표현식으로 함수를 정의하면 -> **변수 호이스팅**이 발생
```javascript
// 함수 참조
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function(x, y) {
  return x - y;
};
```
# 함수 호출과 반환문
* 함수의 매개변수는 함수 몸체 내부에서만 참조할 수 있다. 즉, 매개변수의 스코프는 함수 내부이다.
* 함수는 매개변수의 개수와 인수의 개수가 일치하지 않아도 된다.
  * 인수가 매개변수보다 부족하면, 나머지 매개변수에 대해서는 암묵적으로 undefined
  * 인수가 매개변수보다 많으면, 초과된 인수는 무시되며 모든 인수는 암묵적으로 arguments 객체에 프로퍼티로 보관
```javascript
function add(x, y) {
  console.log(x, y); // 1 2
  return x + y;
}

add(1, 2);
console.log(x, y); // ReferenceError: x is not defined

// 매개변수의 개수 > 인수의 개수 = 나머지 매개변수 undefined
function mul(x, y) {
  console.log(x, y); // 1 undefined
  return x * y;
}
mul(1); // NaN

// 매개변수의 개수 < 인수의 개수 = arguments 에 보관
function sub(x, y) {
  console.log(arguments); // [Arguments] { '0': 3, '1': 2, '2': 1 }
  return x - y;
}
sub(3, 2, 1); // 1
```
### 자바스크립트 문법상의 문제
1. 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
2. 자바스크립트는 **동적 타입 언어**다. 따라서 자바스크립트 함수는 **매개변수의 타입을 사전에 지정할 수 없다.**
* 따라서 함수를 정의할 때, 적절한 인수가 전달되었는지 확인할 필요가 있다.
  1. typeof 연산자를 사용하는 방법
  2. 매개변수에 기본값을 할당하는 방법
  3. 인수가 전달되지 않은 경우 단축 평가를 사용하는 방법
  4. 정적 타입 선언이 가능한 Typescript를 사용하는 방법
```javascript
// 1️⃣ typeof 연산자로 arguments 문제 방지
function add(x, y) {
  if (typeof x !== "number" || typeof y !== "number") {
    throw new TypeError("인수는 모두 숫자(number)값 이어야 합니다.");
  }

  return x + y;
}
console.log(add(1, 2)); // 3
console.log(add(2)); // TypeError: 인수는 모두 숫자(number)값 이어야 합니다.
console.log(add("a", "b")); // TypeError: 인수는 모두 숫자(number)값 이어야 합니다.

// 2️⃣ "단축 평가"로 arguments 문제 방지
function mul(a, b, c) {
  a = a || 1;
  b = b || 1;
  c = c || 1;

  return a * b * c;
}
console.log(mul(1, 2, 3)); // 6
console.log(mul(1, 2)); // 2
console.log(mul(1)); // 1
console.log(mul()); // 1

// 3️⃣ parameter default value 설정으로 argument 문제 방지
// 매개변수 기본값은 매개변수에 인수를 전달하지 않았을 경우와 undefined를 전달한 경우에만 유효하다.
function sub(a = 0, b = 0) {
  return a - b;
}
console.log(sub(10, 9)); // 1
console.log(sub(10)); // 10
console.log(sub()); // 0
```
> 함수는 return 키워드와 표현식(반환값)으로 이뤄진 반환문을 사용해 실행 결과를 함수 외부로 반환할 수 있다.
* 반환문의 2가지 역할
  1. 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나간다. -> 반환문 이후의 문 무시
  2. 반환문은 return 키워드 뒤에 오는 표현식을 평가해 반환한다. -> return 키워드 뒤에 반환값으로 사용할 표현식을 지정하지 않으면 undefined가 반환
* 반환문은 생략 가능하며, 함수 몸체의 마지막 문까지 실행한 후 암묵적으로 undefined를 반환한다.
* 반환문은 함수 몸체 내부에서만 사용할 수 있다. (Node.js 환경에서는 에러 발생 x)
# 참조에 의한 전달과 외부 상태의 변경
> 매개변수도 함수 몸체 내부에서 변수와 동일하게 취급되므로 매개변수 또한 타입에 따라 값에 의한 전달, 참조에 의한 전달 방식을 그대로 따른다.
* 값에 의한 호출
  * 원시 타입의 argument는 값 자체가 복사되어 매개변수에 전달
  * 이 값을 재할당을 통해 변경하더라도 원본은 훼손되지 않는다. (부수 효과 X)
* 참조에 의한 호출
  * 객체 argument는 참조 값이 복사되어 매개변수에 전달
  * 참조 값을 통해 전달한 객체를 변경할 경우 원본이 훼손된다. (부수 효과 O)
  * 객체가 변경될 수 있기 때문에 상태 변화 추적이 어렵다.
```javascript
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = "Kim";
}

// 외부 상태
var num = 100; // 원시 값
var person = { name: "Goeun" }; // 객체

console.log(num); // 100
console.log(person); // { name: 'Goeun' }

changeVal(num, person);

console.log(num); // 100
console.log(person); // { name: 'Kim' }
```
* 해결 방법 중 하나는 객체를 불변 객체로 만들어 사용하는 것
  * 객체의 복사본을 새롭게 생성하는 비용은 들지만 객체를 마치 원시 값처럼 변경 불가능한 값으로 동작하게 만드는 것이다.
  * 객체의 상태 변경이 필요한 경우에는 원본 객체를 완전히 복제하는 깊은 복사를 통해 새로운 객체를 생성하고 재할당을 통해 교체한다.
# 다양한 함수의 형태
## 즉시 실행 함수
> 함수 정의와 동시에 즉시 호출되는 함수이다. 단 한 번만 호출되며 다시 호출할 수 없다.
* 즉시 실행 함수는 반드시 그룹 연산자 (...)로 감싸야 한다.
* 즉시 실행 함수는 함수 이름이 없는 익명 함수를 사용하는 것이 일반적이다.
* 즉시 실행 함수도 일반 함수처럼 값을 반환할 수 있고 인수를 전달할 수도 있다.
```javascript
(function () {
  /// ...
}());
```
## 재귀 함수
> 함수가 자기 자신을 호출하는 것을 재귀 호출이라 한다. 재귀 함수는 반복되는 처리를 위해 사용한다.
## 중첩 함수
> 함수 내부에 정의된 함수를 중첩 함수 또는 내부 함수라 한다.
* 중첩 함수를 포함하는 함수는 외부 함수라 부른다. 중첩 함수는 외부 함수 내에서만 호출할 수 있다.
* 중첩 함수는 자신을 포함하는 외부 함수를 돕는 헬퍼 함수의 역할을 한다.
```javascript
function outer() {
  var x = 1;

  // 중첩 함수
  function inner() {
    var y = 2;

    // 외부 함수의 변수 참조
    console.log(x + y); // 3
  }

  inner();
}

outer();
```
## 콜백 함수
> 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수
* 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수 라고 한다.
* 콜백 함수는 고차 함수에 의해 호출된다.
* 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다. → 그렇기 때문에 고차함수에 콜백함수 전달 시 콜백 함수를 호출하지 않고 함수 자체를 전달해야 한다.
* 수는 일급 객체 이므로 함수의 매개변수를 통해 함수를 전달 가능
* 그로인해, 함수는 더 이상 내부로직에 강력히 의존하지 않고 외부에서 로직의 일부분을 함수로 전달받아 수행하므로 유연한 구조를 갖는다.
```javascript
// 외부에서 전달받은 func 를 n 만큼 반복 호출 - 고차 함수 repeat
function repeat(n, f) {
  for (var i = 0; i < n; i++) {
    f(i);
  }
}

// 콜백 함수 정의 - logAll
var logAll = function (i) {
  console.log(i);
};

// 반복 호출할 함수를 인수로 전달
repeat(5, logAll); // 0 1 2 3 4

// 콜백 함수 정의 - logOdd
var logOdd = function (i) {
  if (i % 2) console.log(i);
};

// 반복 호출할 함수를 인수로 전달
repeat(5, logOdd); // 1 3
```
