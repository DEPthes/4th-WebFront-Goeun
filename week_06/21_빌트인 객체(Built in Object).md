# 자바스크립트 객체의 분류
1. 표준 빌트인 객체: ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공한다. ECMAScript 사양에 정의된 객체이므로 자바스크립트 실행 환경과 관계없이 언제나 사용할 수 있으며, 전역 객체의 프로퍼티로서 제공되기에 전역 변수처럼 언제나 참조할 수 있다.
2. 호스트 객체: ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체를 말한다. 브라우저 환경에서는 클라이언트 사이드 Web API를 호스트 객체로 제공하고, Node.js 환경에서는 Node.js 고유의 API를 호스트 객체로 제공한다.
3. 사용자 정의 객체: 사용자가 직접 정의한 객체를 말한다.
# 표준 빌트인 객체
> Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
* 자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, Function, Error 등 40여 개의 표준 빌트인 객체를 제공한다.
* 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공, 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.
* 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체이다.
```javascript
const strObj = new String("Choi");
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```
* 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공하며, 인스턴스 없이 정적으로 호출할 수 있는 정적 메서드를 제공한다.
```javascript
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5);

// Number.prototype의 프로토타입 메서드인 toFixed
console.log(numObj.toFixed()); // 2

// Number의 정적 메서드인 isInteger
console.log(Number.isInteger(0.5)); // false
```
# 원시값과 래퍼 객체
> 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라 한다.
* 원시값인 문자열, 숫자, 불리언의 원시값에 대해 객체처럼 마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해준다.
* 원시값을 객체처럼 사용하면 자바스크립트 엔진이 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.
```javascript
const str = "Hello";

// 원시 타입인 문자열 -> 래퍼 객체인 String 인스턴스로 변환
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
// 생성되었던 래퍼 객체는 "가비지 컬렉션"의 대상이 된다.
console.log(typeof str); // string
```
* 문자열, 숫자, 불리언, 심벌까지 래퍼 객체를 생성하며, null이나 undefined는 래퍼 객체를 생성하지 않는다. -> null or undefined를 객체처럼 사용하면 에러 발생
# 전역 객체
> 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.
* 브라우저 환경에서는 window가 전역 객체를, Node.js 환경에서는 global이 전역 객체를 가리킨다.
```
📌 globalThis
+ ES11에서 도입된 globalThis는 브라우저 환경과 Node.js 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자다.
+ globalThis는 ECMAScript 표준 사양을 준수하는 모든 환경에서 사용할 수 있다.
```
* 전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체, var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.
  * 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체다.
  * 이 말은 프로토타입 상속 관계상에서 최상위 객체라는 의미가 아닌, 전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 빌트인 객체를 프로퍼티로 소유한다는 것을 말한다.
* 전역 객체의 특징은 다음과 같다.
  * 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
  * 전역 객체의 프로퍼티를 참조할 때, window(또는 global)를 생략할 수 있다.
  * 전역 객체는 표준 빌트인 객체를 프로퍼티로 가진다.
  * 브라우저, Node.js 등 환경에 따라, 추가적인 프로퍼티/메서드를 갖는다.
  * var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.
  * let, const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. ( 즉, window(또는, global) 로 접근할 수 없다.)   
    이 키워드로 선언한 전역 변수는, 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.
  * 모든 자바스크립트 코드는 하나의 전역 객체를 공유한다.
## 빌트인 전역 프로퍼티
> 전역 객체의 프로퍼티를 의미하며, 주로 애플리케이션 전역에서 사용하는 값을 제공한다.
* Infinity
  * Infinity 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.
* NaN
  * NaN 프로퍼티는 숫자가 아님을 나타내는 숫자값 NaN을 갖는다. == Number.NaN 프로퍼티
* undefined
  * undefined 프로퍼티는 원시 타입 undefined를 값으로 갖는다.
## 빌트인 전역 함수
> 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.
* eval
  * eval 함수는 자바스크립트 코드를 나타태는 문자열을 인수로 전달받는다.
  * 전달받은 문자열 코드가 표현식 -> 문자열 코드를 런타임에 평가, 값 생성
  * 전달받은 문자열 코드가 표현식이 아닌 문 -> 문자열 코드를 런타임에 실행
  * 전달받은 문자열 코드가 여러 개의 문 -> 모든 문을 실행한 후 마지막 결과값을 반환
  * 전달받은 문자열 코드가 let, const를 사용한 변수 선언문 -> 암묵적으로 strict mode가 적용
  * eval 함수는 보안에 취약, 처리 속도가 느리므로 **사용을 금지**해야 한다.
* isFinite
  * 전달받은 인수가 정상적인 유한수인지 검사하여 유한수면 true, 무한수면 false를 반환한다.
  * 전달받은 인수의 타입이 숫자가 아닌 경우, 숫자로 타입을 변환한 후 검사를 수행하며 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
* isNaN
  * 전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다.
  * 전달받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입을 변환한 후 검사를 수행한다.
* parseFloat
  * 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.
* parseInt
  * 전달받은 문자열 인수를 정수로 해석하여 반환한다.
  * 전달받은 인수가 문자열이 아니면 문자열로 변환한 다음, 정수로 해석하여 반환한다.
* encodeURI / decodeURI
  ```
  📌 인코딩과 디코딩
  + 인코딩: URI의 문자들을 이스케이프 처리하는 것을 의미
  + 디코딩: 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 복원하는 것을 의미
  ```
  * 이스케이프 처리는 네트워크를 통해 정보를 공유할 때, 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것을 의미한다.
    * URL은 아스키 문자 셋으로 구성되어야 하며 한글을 포함한 대부분의 외국어나 아스키 문자 셋에 정의되지 않은 특수 문자를 포함하지 않아야 한다.
    * 따라서, URL 내부에서 의미를 갖고 있는 문자( %, ?, # )나 한글, 공백 등 또는 시스템에 의해 해석될 수 있는 문자( <, > )를 이스케이프 처리하여 야기될 수 있는 문제를 예방해야 한다.
    * 단, 알파벳, 0~9의 숫자, -, _, ., !, ~, *, ', (, ) 문자는 이스케이프 처리에서 제외
  ```javascript
  // 완전한 URI
  const uri = "http://example.com?name=최고은&job=programmer&student";

  // 원래 URI 형태 -> 이스케이프 처리된 URI 형태로
  const enc = encodeURI(uri);
  console.log(enc); // http://example.com?name=%EC%9C%84%EC%98%81%EB%AF%BC&job=programmer&student

  // 인코딩(이스케이프 처리)된 URI -> 원래 URI 형태로
  const dec = decodeURI(enc);
  console.log(dec); // http://example.com?name=최고&job=programmer&student
  ```
* encodeURIComponent / decodeURIComponent
  * encodeURI / decodeURI 와의 차이점은 URI 전체가 아닌, 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다는 것이다.
  * 따라서, 쿼리 스트링 구분자로 사용되는 ( =, ? , & )까지 인코딩한다.
  ```javascript
  // URI의 쿼리 스트링(구성요소만)
  const uriComp = "name=최고은&job=programmer&student";

  // encodeURIComponent 는 쿼리 스트링 구분자(=, ?, &)까지 인코딩한다.
  let enc = encodeURIComponent(uriComp);
  console.log(enc); // name%3D%EC%B5%9C%EA%B3%A0%EC%9D%80%26job%3Dprogrammer%26student

  let dec = decodeURI(enc);
  console.log(dec); // name%3D최고은%26job%3Dprogrammer%26student

  // encodeURI 는 쿼리 스트링 구분자(=, ?, &)는 인코딩 하지 않는다.
  enc = encodeURI(uriComp);
  console.log(enc); // name=%EC%B5%9C%EA%B3%A0%EC%9D%80&job=programmer&student

  dec = decodeURI(uriComp);
  console.log(dec); // name=최고은&job=programmer&student
  ```
## 암묵적 전역
> 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작한다.
* 전역 객체의 프로퍼티이기 때문에 변수가 아니므로 변수 호이스팅이 발생하지 않는다.
* 또한 변수가 아니기에 delete 연산자로 삭제할 수 있다. 전역 변수는 프로퍼티이지만 delete 연산자로 삭제할 수 없다.
```javascript
console.log(x); // undefined
console.log(y); // ReferenceError

var x = 10;

function foo() {
  y = 20; // window.y = 20;
}
foo();

console.log(x + y); // 30

delete x;
delete y;

console.log(window.x); // 10
console.log(window.y); // undefined
```
