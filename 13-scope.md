# 13장 스코프(Scope)

> 스코프란 모든 식별자(변수이름, 함수이름, 클래스이름)는 `자신이 선언된 위치에 의해` 다른 코드가 식별자 `자신을 참조할 수 있는 유효 범위`가 결정되는 것을 말한다.

```jsx
var x = "global";
function foo() {
  var x = "local";
  console.log(x); // (1) // local
}

foo();
console.log(x); // (2) // global
```

식별자 결정 → `(1)` 과 `(2)` 에서 x 변수를 참조할 때 어떤 변수를 참조할지 결정해야 한다. 

<br>

### 13.2 스코프의 종류

| 구분 | 설명 | 스코프 | 변수 |
| --- | --- | --- | --- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역 | 함수 몸체 내부 | 지역 스코프 | 지역 변수 |

<br>

### 13.2.1 전역과 전역 스코프

전역 스코프 → 코드의 가장 바깥 영역으로 전역 변수는 어디서든지 참조할 수 있다.

<br>

### 13.2.2 지역과 지역 스코프

지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

<br>

### 스코프 예시

```jsx
var x = "전역 x";
var y = "전역 y";

function 외부함수() {
  var z = "외부 지역 z";

  console.log(x); // 전역 x
  console.log(y); // 전역 y
  console.log(z); // 외부 지역 z

  function 내부함수() {
    var x = "내부 지역 x";

    console.log(x); // 내부 지역 x
    console.log(y); // 전역 y
    console.log(z); // 외부 지역 z
  }
  내부함수();
}

외부함수();

console.log(x); // 전역 x
console.log(z); // ReferenceError: z is not defined
```

<br>

(순서 : 스코프 → 변수 참조)

|  | 전역 변수 참조 | 외부지역 변수 참조 | 내부지역 변수 참조 |
| --- | --- | --- | --- |
| 전역 스코프 | O | X | X |
| 외부지역 스코프 | O | O | X |
| 내부지역 스코프 | O | O | O |

‼️ 밖에서는 안을 볼 수 없고 안에서만 밖을 볼 수 있다.

ex) 외부 지역 스코프에서 전역변수 참조는 가능하지만 내부 지역 변수 참조는 불가능!

<br>

## 13.2 스코프 체인

> 스코프가 함수의 중첩에 의해 계층적으로 연결된 것을 스코프 체인이라고 한다.

변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색 한다.

ex) 내부지역 스코프에서  `console.log(y)` 를 출력할 경우 

→ `내부 지역` 스코프에서 변수 y가 선언 되었는가? → No 

→ `외부 지역` 스코프에서 변수 y가 선언 되었는가? → No

→ `전역` 스코프에서 변수 y가 선언 되었는가? → Yes →  `전역 y` 출력
