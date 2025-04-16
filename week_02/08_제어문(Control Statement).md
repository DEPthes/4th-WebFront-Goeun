# 블록문
> 블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부른다.
* 문의 끝에는 세미콜론을 붙이는 것이 일반적이나, 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않는다.
```javascript
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
return a + b;
}
```
# 조건문
> 조건문은 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다. 이때 조건식은 불리언 값으로 평가될 수 있는 표현식이다.
## if...else 문
> if...else 문은 주어진 조건식의 평가 결과인 논리적 참 또는 거짓에 따라 실행할 코드 블록을 결정한다.
> 조건식의 평가 결과가 true일 경우 if 문의 코드 블록이 실행, false일 경우 else 문의 코드 블록이 실행된다.
* if 문의 조건식이 불리언 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 암묵적으로 불리언 값으로 강제 변환된다.
* 코드 블록 내의 문이 하나뿐이라면 중괄호를 생략할 수 있다.
* 대부분의 if...else 문은 삼항 조건 연산자로 바꿔 쓸 수 있다.
## switch 문
> switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다.
> case 문은 상황을 의미하는 표현식을 지정하고 콜론으로 마친다.
* **폴스루**란 switch 문의 표현식의 평가 결과와 일치하는 case 문으로 실행 흐름이 이동하여 문을 실행한 후 switch 문을 탈출하지 않고 switch 문이 끝날 때까지 이후의 모든 case 문과 default 문을 실행하는 현상이다.
* 폴스루를 방지하기 위해 코드 블록에서 탈출하는 역할을 하는 break 문을 사용해야한다.
```javascript
// 윤년을 판별하여 2월의 일수를 계산하는 switch 문 예시
var year = 2000;  // 2000년은 윤년 -> 2월은 29일까지
var month = 2;
var days = 0;

swtich (month) {
	case 1: case 3: case 5: case 7: case 8: case 10: case 12:
		days = 31;
		break;
	case 4: case 6: case 9: case 11:
		days = 30;
		break;
	case 2:
		days = ((year % 4 === 0 || year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
		break;
	default:
		console.log("Invaild month");
}

console.log(days); // 29
```
* 만약 if...else 문으로 해결할 수 있다면 switch 문보다 if...else 문을 사용하는 편이 좋다.
* 조건식이 너무 많아서 if...else 문보다 switch 문을 사용했을 때 가독성이 더 좋다면 switch 문을 사용하는 편이 좋다.
# 반복문
> 반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다.
## for 문
> for 문은 조건식이 거짓으로 평가돌 때까지 코드 블록을 반복 실행한다.   
> for 문 내에 for 문을 중첩해 중첩 for 문을 사용할 수 있다.
```javascript
for (var i = 0; i < 2; i++) {
  console.log(i);
}
// 0
// 1
```
## while 문
> while 문은 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.
> 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 강제 변환하여 논리적 참, 거짓을 구별한다.
```javascript
📌 while 문 -> 반복 횟수가 불명확할 때 주로 사용
📌 for 문 -> 반복 횟수가 명확할 때 주로 사용
```
## do...while 문
> do...while 문은 코드 블록을 먼저 실행하고 조건식을 평가하기 때문에 코드 블록은 무조건 한 번 이상 실행된다.
```javascript
var count = 0;
do {
  console.log(count); // 0 1 2
  count++;
} while (count < 3);
```
# break 문
> break 문은 레이블 문, 반복문, switch 문의 코드 블록을 탈출한다.
> 앞서 말한 문 외에 break 문을 사용하면 SyntaxError가 발생한다.
```javascript
/*
레이블 문이란 식별자가 붙은 문을 말한다.
+ 레이블 문은 프로그램 실행 순서를 제어하는 데 사용한다.
+ switch 문의 case 문과 default 문도 레이블 문이다.
+ 레이블 문을 탈출하려면 break 문에 레이블 식별자를 지정한다.
*/
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log("foo");
// foo라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo; //  foo 레이블 블록문을 탈출한다.
  console.log(2);
}
```
* 중첩된 for 문 외부로 탈출할 때 레이블 문을 사용하면 유용하지만, 그 밖의 경우에는 흐름이 복잡해지기 때문에 권장하지 않는다.
# continue 문
> continue 문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.
* if 문 내에서 실행해야 할 코드가 한 줄이라면 -> 반복문 내에서 continue 문을 사용할 필요는 없다.
* 하지만 if 문 내에서 실행해야 할 코드가 길다면 -> 들여쓰기가 한 단계 더 기퍼지므로, continue 문을 사용하는 편이 가독성이 좋다.
```javascript
// if문 내에서 여러 코드 작성해야 할 경우 -> continue 문을 사용하지 않았을 경우
var arr = [1, 2, 3, 4, 5];
var target = 3;
var count = 0;

for (var i = 0; i < arr.length; i++) {
  // arr[i] 가 target 이하라면 count 증감
  if (arr[i] <= target) {
    count++;
    // code...
    // code...
    // code...
  }
}

// if문 내에서 여러 코드 작성해야 할 경우 -> continue 문을 사용한 경우 -> depth가 하나 줄어들었다.
var arr = [1, 2, 3, 4, 5];
var target = 3;
var count = 0;

for (var i = 0; i < arr.length; i++) {
  // arr[i] 가 target 초과이면 count 증감하지 않는다.
  if (arr[i] > target) continue;

  count++;
  // code...
  // code...
  // code...
}
```
