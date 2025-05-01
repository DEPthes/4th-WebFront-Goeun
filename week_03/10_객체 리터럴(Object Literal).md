# 객체
> 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성된다.
```javascript
var counter {
  num: 0, // 프로퍼티
  // 메서드
  increase: function() { 
    this.num++;
  }
};
```
* 자바스크립트의 함수는 일급 객체이므로 값으로 취급할 수 있으며, 따라서 함수도 프로퍼티 값으로 사용할 수 있다.
* 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부른다.
> 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지원한다.
1. 객체 리터럴
2. Object 생성자 함수
3. 생성자 함수
4. Object.create 메서드
5. 클래스(ES6)
# 객체 리터럴에 의한 객체 생성
> 객체 리터럴은 객체를 생성하기 위한 표기법이다.
* 객체 리터럴은 중괄호( {...} ) 내에 0개 이상의 프로퍼티를 정의한다.
* 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.
```javascript
var person = {
  name: "Goeun",
  sayHello: function () {
    console.log(`Hello My name is ${this.name}`);
  },
};

console.log(typeof person); // object
console.log(person); // { name: 'Goeun', sayHello: f }
```
* 객체 리터럴의 중괄호는 코드 블록을 의미하지 X, 객체 리터럴은 값으로 평가되는 표현식이기 때문에 중괄호 뒤에 세미콜론을 붙인다.
# 프로퍼티
* 프로퍼티를 나열할 때는 쉼표로 구분한다.
* 프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
* 프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값
> 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자 역할을 한다.
* 프로퍼티 키는 문자열이므로 따옴표로 묶어야 한다. 하지만 식별자 네이밍 규칙을 준수하는 이름은 따옴표를 생략할 수 있다.
```javascript
var person = {
	firstName: 'Goeun',  // 식별자 네이밍 규칙을 준수한 프로퍼티 키
	'last-name': 'Choi', // 식별자 네이밍 규칙을 준수하지 않은 프로퍼티 키 ( 따옴표를 사용해 문자열 형태 유지 )
  last-name: 'Choi'    // SyntaxError: Unexpected token ( 식별자 네이밍 규칙을 준수하지 않은 프로퍼티 키 ( 따옴표를 사용하지 않을 경우 - 표현식으로 해석 ) )
}
```
* 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.
* 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.
# 메서드
> 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부른다.   
> 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.
# 프로퍼티 접근
1. 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법
2. 대괄호 프로퍼티 접근 연산자( [...] )를 사용하는 대괄호 표기법
```javascript
var person = {
  name: "Goeun",
};

console.log(person.name); // "Goeun" ( 마침표 표기법 )
console.log(person["name"]); // "Goeun" ( 대괄호 표기법 )

// ❗ 대괄호 프로퍼티 접근 연산자 내에 문자열 형태가 아닌 프로퍼티 키로 사용하면 자바스크립트 엔진은 "식별자"로 해석
console.log(person[name]); // ReferenceError: name is not defined

// ❗ 객체에 존재하지 않는 프로퍼티에 접근시 -> undefined
console.log(person.age); // undefined
```
* 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름이라면 반드시 대괄호 표기법을 사용해야 한다.
* 프로퍼티 키가 숫자로 이루어진 문자열일 경우 따옴표를 생략 가능, 그 외 대괄호 내에 들어가는 프로퍼티 키는 따옴표로 감싼 문자열이어야 한다.
```javascript
var person = {
  1: 10
};

person.1; // SyntaxError: Unexpected number
person.'1'; // SyntaxError: Unexpected string
person[1]; // 10 (person[1] -> person['1']
person['1']; // 10
```
# 프로퍼티 동적 생성 & 삭제
* 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.
* delete 연산자는 객체의 프로퍼티를 삭제한다.
* 이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다.
```javascript
var person = {
  name: "Goeun",
};

person.age = 100; // { age: 100 } 프로퍼티 동적 생성
console.log(person); // { name: 'Goeun', age: 100 }

delete person.age; // age 라는 프로퍼티 키가 있고 -> 해당 프로퍼티 삭제
delete person.job; // job 이라는 프로퍼티 키는 없음 -> 그럼에도 delete 연산시 에러 발생 X

console.log(person); // { name: 'Goeun' }
```
# ES6에서 추가된 객체 리터럴의 확장 기능
## 프로퍼티 축약 표현
* ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다.
```javascript
let x = 1, y = 2;
const obj = {x, y}; // 프로퍼티 키는 변수 이름으로 자동 생성
console.log(obj); // {x: 1, y: 2)
```
## 계산된 프로퍼티 이름
* 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있다.
* 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 한다.
* ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있다.
```javascript
const prefix = "prop";
let i = 0;

// "객체 리터럴 내부"에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // { 'prop-1': 1, 'prop-2': 2, 'prop-3': 3 }
```
## 메서드 축약 표현
* ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다.
* ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수(ES5)와 다르게 동작한다.
```javascript
const obj = {
  name: "Goeun",

  // 메서드 축약 표현 ( 함수 선언식 필요 X )
  sayHi() {
    console.log(`Hi! ${this.name}`);
  },
};

obj.sayHi(); // Hi! Goeun
```
