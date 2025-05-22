# this 키워드
> this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.   
> this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
* this는 자바스크립트 엔진에 의해 암묵적으로 생성되며 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.
* this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
```
📌 this 바인딩
+ 바인딩: 식별자와 값을 연결하는 과정
+ this 바인딩: this(키워드로 분류되지만 식별자 역할을 한다)와 this가 가리킬 객체를 바인딩하는 것
```
* 객체 리터럴의 메서드 내부에서 this는 메서드를 호출한 객체다.
```javascript
// 객체 리터럴
const circle = {
  radius: 5,

  getDiameter() {
    // 여기서 this 는 메서드를 호출한 객체 (circle)
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter());  // 10
```
* 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스다.
```javascript
// 생성자 함수
function Circle(radius) {
  // 여기서 this는 생성자 함수 Circle이 생성할 인스턴스 (circle)
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 여기서 this는 생성자 함수 Circle이 생성할 인스턴스 (circle)
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter());  // 10
```
* this는 코드 어디에서든 참조 가능하다. 전역에서도 함수 내부에서도 참조할 수 있다.
* 하지만 this의 본질은 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수의 개념이다.
* 따라서 객체가 아닌 일반 함수 내부의 this는 필요가 없으므로, strict mode 에서 this를 참조할 경우, undefined 를 반환한다.
# 함수 호출 방식과 this 바인딩
> this 바인딩은 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.
* 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정하지만. this 바인딩은 함수 호출 시점에 결정된다.
* 함수를 호출하는 방식은 다음과 같다.
1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
## 일반 함수 호출
> 기본적으로 this에는 전역 객체가 바인딩된다.
* 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.
* strict mode 에서 this를 참조할 경우는 undefined가 바인딩된다.
```javascript
function foo() {
  console.log(`전역 함수 foo의 this : ${this}`);    // window

  function bar() {
    console.log(`중첩 함수 bar의 this : ${this}`);  // window
  }

  bar();
}
foo();
```
```javascript
// 전역 변수 value (전역 객체 프로퍼티)
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(`foo 메서드의 this : ${this}`); // {value: 100, foo: f}
    console.log(`foo 메서드의 this가 가리키는 객체의 value : ${this.value}`); // 100

    // 메서드 내 정의한 중첩 함수
    function bar() {
      console.log(`메서드 내 중첩 함수 bar의 this : ${this}`); // window
      console.log(`중첩 함수 bar의 this가 가리키는 객체의 value : ${this.value}`); // 1
    }

    // 메서드 내 중첩 함수를 일반 함수로 호출하면 중첩 함수 내부의 this에는 전역 객체가 바인딩
    bar();
  },
};

obj.foo();
```
```javascript
// 전역 변수 value (전역 객체 프로퍼티)
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(`foo 메서드의 this : ${this}`); // {value: 100, foo: f}
    console.log(`foo 메서드의 this가 가리키는 객체의 value : ${this.value}`); // 100

    // 메서드 내 콜백 함수
    setTimeout(function () {
      console.log(`callback 함수의 this : ${this}`); // window
      console.log(`callback 함수의 this가 가리키는 객체의 value : ${this.value}`); // 1
    }, 100);
  },
};

obj.foo();
```
* 중첩 함수나 콜백 함수의 경우, 용도가 일반적으로 외부 함수를 돕는 헬퍼 함수의 역할을 하므로, this가 가리키는 객체가 전역 객체일 경우는 헬퍼 함수의 역할을 하기 힘들다.
* 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다.
  * 메서드의 this 바인딩할 객체를 변수에 할당하는 방법
  ```javascript
  // 전역 변수 value (전역 객체 프로퍼티)
  var value = 1;
  
  const obj = {
    value: 100,
    foo() {
      // 콜백 함수에 바인딩할 obj 객체를 가리키는 this를 변수 that에 할당
      const that = this;
  
      setTimeout(function () {
        console.log(`callback 함수의 this가 가리키는 객체의 value : ${that.value}`); // 100
      }, 100);
    },
  };
  
  obj.foo();
  ```
  * Function.prototype.apply/call/bind 메서드를 이용한 this를 명시적으로 바인딩하는 방법
  ```javascript
  // 전역 변수 value ( 전역 객체 프로퍼티 )
  var value = 1;
  
  const obj = {
    value: 100,
    foo() {
      // Function.prototype.bind 메서드로 obj 객체를 가리키는 this를 콜백함수에 명시적으로 바인딩
      setTimeout(
        function () {
          console.log(`callback 함수의 this가 가리키는 객체의 value : ${this.value}`); // 100
        }.bind(this),
        100,
      );
    },
  };
  
  obj.foo();
  ```
  * 화살표 함수를 사용한 this 바인딩 일치 방법
  ```javascript
  var value = 1;
  
  const obj = {
    value: 100,
    foo() {
      // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
      setTimeout(() => console.log(this.value), 100); // 100
    }
  };
  
  obj.foo();
  ```
## 메서드 호출
> 메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.
```javascript
const person = {
  name: "Choi",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    // getName 메서드는 person 객체에 포함된 것이 아닌, 독립적으로 존재하는 별도의 객체 개념
    return this.name;
  },
};

console.log(person.getName());  // Choi
```
* 메서드는 프로퍼티에 바인딩된 함수다.
  * 즉, 메서드는 특정 객체에 포함된 것이 아닌, 독립적으로 존재하는 별도의 객체이다.
  * 다른 객체의 프로퍼티에 할당이 가능하며, 일반 변수에 할당하여 일반 함수로 호출도 가능하다.
```javascript
const person = {
  name: "Choi",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩
    // getName 메서드는 person 객체에 포함된 것이 아닌, 독립적으로 존재하는 별도의 객체 개념
    return this.name;
  },
};

const anotherPerson = {
  name: "Lee",
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당 (getName 메서드는 독립적인 객체이기 때문)
anotherPerson.getName = person.getName;

// getName을 호출한 객체는 이 시점에선 person이 아닌 anotherPerson이다.
console.log(anotherPerson.getName());  // Lee

// getName을 getName 변수에 할당 (getName 메서드는 독립적인 객체이기 때문)
const getName = person.getName;

// 일반 함수로 호울된 getName 함수 내의 this.name은 브라우저 환경에서 window.name과 같다.
// window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ' '이다.
console.log(getName());  // ' '
```
* 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.
```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입에 getName 메서드 할당
Person.prototype.getName = function () {
  return this.name;
};

// me 인스턴스 생성
const me = new Person("Choi");

// 이 시점에서 getName 메서드를 호출한 주체는 me 객체
console.log(me.getName()); // Choi

Person.prototype.name = "Lee";

// 이 시점에서 getName 메서드를 호출한 주체는 Person.prototype 객체
console.log(Person.prototype.getName()); // Lee
```
## 생성자 함수 호출
> 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.
* 생성자 함수는 객체(인스턴스)를 생성하는 함수다.
* 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.
  * 만약 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 동작한다.
```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}
// 함수는 특별히 다른 객체를 명시적으로 return하지 않는 이상, this가 가리키던 그 새로 만들어진 객체를 자동으로 반환

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.radius); // 5
console.log(circle2.radius); // 10

// 함수 내부의 this가 브라우저 환경의 전역 객체인 window를 가리키게 됨
// window.radius = 15;
// 일반 함수는 명시적인 return 문이 없으면 기본적으로 undefined를 반환

const circle3 = Circle(15); 
console.log(circle3); // undefined
console.log(radius); // 15
```
## Function.prototype.apply / call / bind 메서드에 의한 간접 호출
> apply, call, bind 메서드는 Function.prototype의 메서드다. -> 모든 함수가 상속받아 사용할 수 있다.
* Function.prototype.apply(this로 사용할 객체, 인수 리스트의 배열 또는 유사 배열 객체
* Function.prototype.call(this로 사용할 객체, (,)로 구분된 인수 리스트
* Function.prototype.bind(this로 사용할 객체)
1. apply, call
* 본질적인 기능은 함수를 호출하는 것이다.
  * 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.
  * 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.
```javascript
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = {a:1);

// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee:f, Symbol(Symbol.iterator): f]
// {a:1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee:f, Symbol(Symbol.iterator): f]
// {a:1}
```
2. bind
* apply, call 메서드와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달한다.
* 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.
```javascript
const person = {
  name: "Choi",
  foo(callback) {
    // bind 를 적용하지 않는다면, foo 메서드 내부에 콜백 함수에 정의된 this는 전역 객체(window 또는 global)를 가리킨다.
    // 전역 객체에는 name 프로퍼티가 없기 때문에, 원래는 undefined 를 출력하는 것이 맞다.
    // 하지만, Function.prototype.bind 메서드로 콜백 함수의 주체를 person 객체로 동적 바인딩 해주었다.
    // 때문에 person 객체의 name 프로퍼티에 접근할 수 있게 되었다.
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`안녕하세요. ${this.name}입니다.`); // 안녕하세요. Choi입니다.
});
```
### 정리

| 함수 호출 방식                                                      | this 바인딩 대상                                                            |
| :------------------------------------------------------------------ | :---------------------------------------------------------------------------- |
| 일반 함수 호출                                                      | 전역 객체 (브라우저: window, Node.js: global)                           |
| 메서드 호출                                                         | 메서드를 호출한 객체                                                          |
| 생성자 함수 호출 (new 연산자와 함께)                                | 생성자 함수가 새로 생성할 인스턴스                                            |
| Function.prototype의 apply, call, bind 메서드에 의한 간접 호출 | apply, call, bind 메서드의 첫 번째 인수로 전달한 객체                 |
