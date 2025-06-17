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
