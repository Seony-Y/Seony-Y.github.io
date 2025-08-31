---
title : var, let, const 를 중복 선언 허용, 스코프, 호이스팅 관점에서 서로 비교하기
date : 2025-09-01 00:48:00 +9:00
categories : [Javascript]
tags : [Javascript]
---

## 중복 선언 허용

- var은 중복 선언이 허용

```javascript
var user = 1;
console.log(user); // 1
var user = 2;         // JS에 의해 없는것 처럼 동작. 에러 또한 발생하지 않음
console.log(user); // 2
// 의도치 않은 변수값이 재할당되어 변경될 수 있다.

let, const는 중복 선언이 허용되지 않음
let user;
let user ;  // SyntaxError: identifier 'user' has already been declared
```

## 스코프
- 함수 레벨 스코프 : 함수를 기준으로만 적용되는 스코프
- 블록 레벨 스코프 : 코드블록({} 중괄호)을 기준으로 적용되는 스코프
- var로 함수 내부에서 선언한 변수는 지역 스코프, 함수 외부에서 선언한 변수는 전역 변수이다. 블록 기준으로 스코프가 생기지 않기 때문에 블록 밖에서 접근 가능
- if, for, while, switch 등 다양한 상황에서 전역 변수로 사용할 수 있다.

```javascript
var c = 4;

if(true){
    var a = 11;
    console.log(a); // 11
}
function fnc(){
    var b = 55;
      var c = 44;
    console.log(b); // 55
}
console.log(a);     // 11
console.log(b);     // ReferenceError: b is not defined
console.log(c);     // 44

let, const로 선언한 변수는 블록 레벨 스코프이다. 코드 블록 내에서만 유효하며 블록 외부에서는 참조할 수 없다.
let a = 11;         // 전역 변수

if(true){
    let b = 22;        // 지역 변수
    console.log(b); // 22
}
console.log(a);     // 11
console.log(b);     // ReferenceError: a is not defined
```

## 블록 레벨 스코프의 중첩

```javascript
let i = 10; // 전역 스코프
function foo(){
    let i = 100; // 함수 레벨 스코프
    
    for(let i = 1; i< 3; i++){ // 블록 레벨 스코프
        console.log(i); // 1 2
    }
    console.log(i); // 100
}

foo();

console.log(i); // 10
```

## 호이스팅
- var로 선언한 변수는 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작
- var로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단 할당문 이전에 변수를 참조하면 undefined를 반환

```javascript
// 이 위치로 호이스팅에 의해 a변수가 선언
// 변수 a는 undefined로 초기화
console.log(a); // undefined

// 변수에 값을 할당
a = 50;

console.log(a); // 50

// 호이스팅에 의해 상단에 선언
var a;
```

- let, const로 선언한 변수는 호이스팅이 발생하지 않은 것처럼 동작
- 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대라고 한다.

```javascript
// 런타임 이전에 선언 단계가 실행 아직 변수는 초기화 되지 않음
// 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없음
console.log(a); // ReferenceError: a is not defined

let a;
console.log(a); // undefined

a = 1;
console.log(a); // 1
```