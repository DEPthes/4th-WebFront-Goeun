# var 키워드로 선언한 변수의 문제점
## 1. 변수 중복 선언 허용
* 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.
```javascript
var x = 1; // x변수 선언 & 초기화 동시에
var y = 1; // y변수 선언 & 초기화 동시에

var x = 100; // 선언 문 O, 초기화 문 O (var 키워드가 없는 것처럼 동작)
var y; // 선언 문은 O, 초기화 문 X (암묵적 무시)

console.log(x); // 100
console.log(y); // 1
```
## 2. 함수 레벨 스코프
* 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.
* 함수 레벨 스코프는 전역 변수를 남발할 가능성을 높인다. -> 의도치 않게 전역 변수가 중복 선언되는 경우 발생
```javascript
var x = 1;

if(true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 중복 선언 문제 발생
  var x = 10;
}

console.log(x); // 10
```
## 3. 변수 호이스팅
* var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.
* 변수 선언문 이전에 변수를 참조하는 것은 에러가 발생하지는 않지만 프로그램의 흐름상 맞지 않을뿐더러 가독성을 떨어뜨리고 오류를 발생시킬 여지를 준다.
```javascript
console.log(foo); // undefined << foo 변수 암묵적 선언 & 초기화 (호이스팅)

foo = 123; // foo 변수 값 할당

console.log(foo); // 123

var foo;
```
# let 키워드
> var 키워드의 단점을 보완하기 위해 ES6에서 새로운 변수 선언 키워드인 let과 const를 도입했다.
## 1. 변수 중복 선언 금지
* let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.
```javascript
// var 변수 = 중복 선언 허용 O
var foo = 123;
var foo = 456;
console.log(foo);

// let 변수 = 중복 선언 허용 X
let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
console.log(bar);

// 선언 단계 에서 체크하므로 위에서 var 변수인 foo에 대한 출력문 실행이 진행 안됨
```
## 2. 블록 레벨 스코프
* let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
```javascript
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```
## 3. 변수 호이스팅
* let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
```javascript
console.log(foo); // ReferenceError: foo is not defined
let foo;
```
* let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.
  -> 런타임 이전에 자바스크립트 엔진에 의해 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행
* 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다.
* 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간 -> 일시적 사각지대
```javascript
console.log(foo); // 일시적 사각지대(TDZ) - ReferenceError: Cannot access 'foo' before initialization ( 사실상, 여기서 프로그램 종료 )

let foo; // 변수 선언문에서 초기화 단계가 실행
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행
console.log(foo); // 1
```
![let 초기화](https://github.com/user-attachments/assets/c9876c19-68a5-46b6-82bb-14a8abda0568)
## 4. 전역 객체와 let
* let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
* let 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다.
```javascript
// 브라우저 환경 가정
var a = 1;

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티 O
console.log(window.a); // 1
console.log(a); // 1

let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 X
console.log(window.x); // undefined
console.log(x); // 1
```
# const 키워드
> const 키워드는 상수를 선언하기 위해 사용한다. (단, 반드시 상수만을 위해 사용하지는 않는다.)
* let 키워드와 특징이 대부분 동일하므로 let 키둬드와 다른 점에 초점
## 1. 선언과 초기화
* const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.
```javascript
const foo;  // SyntaxError: Missing initializer in const declaration
```
## 2. 재할당 금지
* var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지된다.
```javascript
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```
## 3. 상수
* const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. (원시 값은 재할당 없이 값을 변경할 수 있는 방법 X)
* 상수는 재할당이 금지된 변수를 말한다.
* 상수는 프로그램 전체에서 공통적으로 사용하므로 나중에 값이 변경되면 상수만 변경하면 되기 때문에 유지보수성이 대폭 향상된다.
* 일반적으로 상수의 이름은 대문자로 선언, 여러 단어로 이뤄진 경우에는 스네이크 케이스(_)로 표현한다.
```javascript
// TAX_RATE 라는 상수(constant)를 적용하므로써, 코드의 가독성이 증가한다.
const TAX_RATE = 0.1;

let preTaxPrice = 100;
let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice); // 110
```
## 4. const 키워드와 객체
* const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다. (변경 가능한 값인 객체는 재할당 없이도 직접 변경 가능)
```javascript
const person = {
  name: "Choi",
};

// 객체는 변경 가능한 값(mutable value) == 재할당 없이 변경 가능
person.name = "Goeun";
console.log(person.name); // { name: "Goeun" };
```
* const 키워드는 재할당을 금지할 뿐 **불변을 의미하지는 않는다.**
  -> 새로운 값을 재할당하는 것은 불가능, 프로퍼티 동적 생성과 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능
# var vs. let vs. const
* 변수 선언 -> 기본적으로 const (의도치 않은 재할당을 방지하기 때문에 안전)
* 재할당이 필요한 경우 -> let (변수의 스코프는 최대한 좁게 만들기)
