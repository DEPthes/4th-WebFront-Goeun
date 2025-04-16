# 타입 변환
> 개발자가 의도적으로 값의 타입을 변환하는 것 -> 명시적 타입 변환 혹은 타입 캐스팅
> 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것 -> 암묵적 타입 변환 혹은 타입 강제 변환
* 원시 값은 변경 불가능한 값이므로 명시적 타입 변환이나 암묵적 타입 변환이 원시 값을 변경하는 것은 아니다.
* 기존의 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.
# 암묵적 타입 변환
> 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다.
## 문자열 타입으로 변환
* 문자열 연결 연산자의 모든 피연산자는 코드의 문맥상 모두 문자열 타입이어야 하므로 문자열 타입으로 암묵적 타입 변환한다.
```javascript
// 심벌 타입
(Symbol()) + '';      // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + '';            // "[object Object]" (빈 객체의 toString() 은 [object Object]를 반환)
Math + '';            // "[object Math]" (Math 객체의 toString() 메서드는 기본 Object.prototype.toString()을 상속받아 [object Math]를 반환)
// Object.prototype은 모든 객체가 기본적으로 연결되어 있는 최상위 프로토타입 (객체는 다른 객체를 참조할 수 있는데, 이 참조하는 객체를 프로토타입)
[] + '';              // "" (배열의 toString() 메서드는 배열 요소들을 쉽표로 구분한 문자열로 반환)
[10, 20] + '';        // "10,20"
(function(){}) + '';  // "function(){}" (함수 객체를 문자열로 변환하면 함수의 소스 코드가 문자열로 반환됨)
Array + '';           // "function Array() { [native code] }" (Array는 내장함수로, 자바스크립트 내장 함수는 구현이 자바스크립트가 아니기 때문에 함수 소스 코드가 [native code]로 표시됨)
```
## 숫자 타입으로 변환
* 산술 연산자의 역할은 숫자 값을 만드는 것이기에 산술 연산자의 피연산자는 숫자 타입으로 암묵적 타입 변환한다.
```javascript
// 문자열 타입
+""; // 0
+"string"; // NaN

// null 타입
+null; // 0

// undefined 타입
+undefined; // NaN

// 심벌 타입
+Symbol(); // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}; // NaN
+[]; // 0
+[10, 20]; // NaN
+function () {}; // NaN
```
* 빈 문자열(''), 빈 배열([]), null, false -> 0
* true -> 1
* 객체, 빈 배열이 아닌 배열, undefined -> NaN
## 불리언 타입으로 변환
* 자바스크립트 엔진은 불리언 값이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다.
```javascript
📌 자바스크립트 엔진이 Falsy 값으로 평가하는 값
false
undefined
null
0, -0
NaN
'' (빈 문자열)
```
* Falsy 값 외의 모든 값은 Truthy 값이다.
# 명시적 타입 변환
1. 표준 빌트인 생성자 함수 -> String(), Number(), Boolean()을 new 연산자 없이 호출하는 방법
2. 빌트인 메서드를 사용하는 방법
3. 암묵적 타입 변환을 이용하는 방법
## 문자열 타입으로 변환
```javascript
// 1. String 생성자 함수를  new 키워드 없이 호출하는 방법
String(NaN);            // "NaN"
String(Infinity);       // "Infinity"

// 2. Object.prototype.toString 메서드를 사용하는 방법
(1).toString();       // "NaN";
(Infinity).toString();  // "Infinity"

// 3. 문자열 연결 연산자를 이용하는 방법
1 + '';                 // "1"
```
## 숫자 타입으로 변환
```javascript
// 1. Number 생성자 함수를 new 키워드 없이 호출하는 방법
Number("-1"); // -1
Number("10.53"); // 10.53
Number(true); // 1

// 2. parseInt, parseFloat 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
parseInt("10.53"); // 10
parseFloat("10.53"); // 10.53

// 산술 연산자를 사용하는 방법
+"0"; // 0
false * 1; // 0
```
## 불리언 타입으로 변환
```javascript
// 1. Boolean 생성자 함수를 new 키워드 없이 호출하는 방법
Boolean('x');       // true
Boolean('');        // false
Boolean('false');   // true

Boolean(NaN);       // false
Boolean(Infinity);  // true
Boolean(null);      // false
Boolean(undefined); // false

Boolean({});        // true
Boolean([]);        // true

// 2. !(부정 논리 연산자를 두 번 사용하는 방법
!!'x';  // true ( !(!'x') === !(false) -> true )
```
# 단축 평가
> 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.
> 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략한다.
## 논리 연산자를 사용한 단축 평가
* 논리곱(&&) 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다.
* 논리합(||) 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다.
```javascript
// 논리합(||) 연산
"Cat" || "Dog"; // "Cat"
false || "Dog"; // "Dog"
"Cat" || false; // "Cat"

// 논리곱(&&) 연산
"Cat" && "Dog"; // "Dog"
false && "Dog"; // "false"
"Cat" && false; // "false"
```
1. 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하게 쓰인다.
```javascript
// 객체는 { 키 : 값 } 으로 구성된 프로퍼티(property)의 집합
// 객체를 가리키기를 기대하는 변수의 값이 객체가 아닌 null 이나 undefined 인 경우, 객체 참조시 TypeError가 발생 -> 프로그램 강제 종료

// 👎
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null

// 👍
// elem이 null 또는 undefined 같은 "Falsy 값"이면 elem 값으로 평가
// elem이 "Truthy 값"이면 elem.value 로 평가
var elem = null;
var elem = elem && elem.value; // null
```
2. 함수 매개변수에 기본값을 설정할 때 유용하게 쓰인다.
```javascript
// 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined 가 할당

// 👎 인수를 전달하지 않을 경우
function getStringLength(str) {
  return str.length;
}
getStringLength(); // TypeError: Cannot read property 'length' of undefined

// 👍 단축 평가를 사용한 매개변수의 기본값 설정 (왼쪽 값이 undefined로 falsy 값이면 ''로 대체)
function getStringLength(str) {
  str = str || "";
  return str.length;
}
getStringLength(); // 0

// 👍 Es6의 매개변수 default parameter 설정 (인자가 명시적으로 전달되면, 기본값 무시. 인자가 undefined일 때만 기본값 적용)
function getStringLength(str = "") {
  return str.length;
}
getStringLength(); // 0
```
## 옵셔널 체이닝 연산자
> 연산자 ?.   
> 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 찹조를 이어간다.   
> ex) obj?.prop1?.prop2 일때 obj가 null 또는 undefined면 obj?.prop1에서 undefined 반환 → prop1 접근 중단   
> 그렇지 않으면 obj.prop1 접근 → 만약 obj.prop1이 null 또는 undefined면 obj?.prop1?.prop2에서 undefined 반환 → prop2 접근 중단   
> 그렇지 않으면 obj.prop1.prop2 값을 반환
* 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 안전하게 참조할 때 유용하게 쓰인다.
```javascript
// 논리곱(&&) 연산자 vs 옵셔널 체이닝 연산자

// 👎 논리곱(&&) 연산자 = 좌항 피연산자가 Falsy값이면, 좌항 피연산자를 그대로 반환한다. (단, 0 또는 ''은 객체로 평가될 때도 있다.)
var str = "";
var length = str && str.length; // ''

// 👍 옵셔널 체이닝 = 좌항 피연산자가 Falsy값이라도 null 또는 undefined만 아니면, 우항의 프로퍼티를 참조한다.
var str = "";
var length = str?.length; // 0
```
## null 병합 연산자
> 연산자 ??
* 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
* 변수에 기본값을 설정할 때 유용하게 쓰인다.
```javascript
// 논리합(||) 연산자 vs null 병합 연산자

// 👎 논리합(||) 연산자 = 좌항의 피연산자가 Falsy값이면, 우항의 피연산자를 반환한다. (단, 0 이나 ''이 기본값으로서 유효하다면 예기치 않은 동작이 발생!)
var foo = "" || "default string"; // "default string"

// 👍 null 병합 연산자 = 좌항의 피연산자가 Falsy값이라도 null 또는 undefined가 아니면, 좌항의 피연산자를 그대로 반환한다.
var foo = "" ?? "default string"; // ''
```
