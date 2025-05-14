# 일급 객체
> 다음 조건을 만족하는 객체를 일급 객체라 한다. 자바스크립트의 함수는 다음 조건을 모두 만족하므로 일급 객체다.
1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.   
* 일급 객체로서 함수가 가지는 가장 큰 특징
  * 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로 사용할 수도 있다.
  * 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다.
  * 일반 객체에는 없는 함수 객체만의 함수 고유의 프로퍼티를 소유한다.
# 함수 객체의 프로퍼티
> arguments, caller, length, name, prototype 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.
## arguments 프로퍼티
> arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다. (함수 외부에서 참조 불가)
* 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체를 참조
* 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
  * 함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급   
    -> 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언, undefined로 초기화된 이후 인수 할당
  * 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우 -> 인수가 전달되지 않은 매개변수는 undefined로 초기화된 상태 유지
  * 매개변수의 개수보다 인수를 더 많이 전달한 경우 -> 초과된 인수는 무시
* 초과된 인수는 버려지는 것이 아닌 arguments 객체의 프로퍼티로 보관된다.
  * arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타낸다.
  * arguments 객체의 length 프로퍼티는 인수의 개수를 가리킨다.
* arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.
```javascript
function multiply(x, y) {
  console.log(arguments);
  console.log(`length : ${arguments.length}`);
  return x + y;
}

console.log(multiply());
/*
[Arguments] {}
length : 0
NaN
*/

console.log(multiply(1));
/*
[Arguments] { '0': 1 }
length : 1
NaN
*/

console.log(multiply(1, 2));
/*
[Arguments] { '0': 1, '1': 2 }
length : 2
3
*/

console.log(multiply(1, 2, 3, 4, 5));
/*
[Arguments] { '0': 1, '1': 2, '2': 3, '3': 4, '4': 5 }
length : 5
3 
*/
```
* arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체다. -> length 프로퍼티를 가진 객체로 for문으로 순회할 수 있는 객체
* 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다.
## length 프로퍼티
> 함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
```javascript
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```
* arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고, 함수 객체의 length 프로퍼티는 매개변수의 개수를 가리킨다.
## name 프로퍼티
> 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다.
* 익명 함수 표현식의 경우   
  ES5 -> 빈 문자열을 값으로 갖는다.   
  ES6 -> 함수 객체를 가리키는 식별자를 값으로 갖는다.
```javascript
// 기명 함수 표현식
const namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
// ES5: 빈 문자열('')
// ES6: 함수 객체를 가리키는 식별자
const anonymousFunc = function () {};
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name); // bar
```
## __proto__ 접근자 프로퍼티
* 모든 객체는 [[ Prototype ]] 이라는 내부 슬롯을 갖는다.
* [[ Prototype ]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
> __proto__ 프로퍼티는 [[ Prototype ]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이다.
* __proto__ 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있다.
```javascript
const obj = { a: 1 };

// 객체 리터럴 형태로 생성한 객체의 프로토타입 객체는 Object.prototype
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.property의 메서드이다. (인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true 반환, 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환)
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("__proto__")); // false
```
## prototype 프로퍼티
> prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티
* 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.
```javascript
console.log(function () {}.hasOwnProperty("prototype")); // true  (함수 객체는 prototype 프로퍼티를 소유)
console.log({}.hasOwnProperty("prototype")); // false (일반 객체는 prototype 프로퍼티를 소유하지 X)
```
```
