---
layout: single
title: "타입 변환과 단축 평가"
categories: DeepDiveJS
tag: [JavaScript]
toc: true
author_profile: false
sidebar:
  nav: "counts"
---

# 타입 변환과 단축 평가

## 타입 변환이란?

값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있다.

개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환<sup>explicit coercion</sup>** 또는 **타입 캐스팅<sup>type casting</sup>**이라 한다.

```javascript
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다.

이를 **암묵적 타입 변환<sup>implicit coercion</sup>** 또는 **타입 강제 변환<sup>type coercion</sup>**이라 한다.

```javascript
var x = 10;

// 암묵적 타입 변환
// 문자열 열결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + "";
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

## 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다.

```javascript
// 피연산자가 모두 문자열 타입이어야 하는 문맥
"10" + 2; // '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * "10"; // 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0; // true
if (1) {
}
```

## 명시적 타입 변환

개발자 의도에 따라 명시적으로 타입을 변경하는 방법은 다양하다. 표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법과 빌트인 메서드를 사용하는 방법, 그리고 암묵적 타입 변환을 이용하는 방법이 있다.

### 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.

1. **String 생성자 함수를 new 연산자 없이 호출**
2. **Object.prototype.toString 메서드를 사용**
3. **문자열 연결 연산자 이용**

```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // "true"
String(false); // "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString(); // "1"
NaN.toString(); // "NaN"
Infinity.toString(); // "Infinity"
// 불리언 타입 => 문자열 타입
true.toString(); // "true"
false.toString(); // "false"
```

### 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 다음과 같다.

1. **Number 생성자 함수를 new 연산자 없이 호출하는 방법**
2. **parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)**
3. **+ 단항 산술 연산자를 이용하는 방법**
4. **\* 산술 연산자를 이용하는 방법**

```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
Number("0"); // 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
parseInt("0"); // 0
parseFloat("10.53"); // 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
+"0"; // 0
+"-1"; // -1
+"10.53"; // 10.53

// 4. * 산술 연산자를 이용하는 방법
"0" * 1; // 0
"-1" * 1; // -1
```

### 불리언 타입으로 변환

불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법은 다음과 같다.

1. **Boolean 생성자 함수를 new 연산자 없이 호출하는 방법**
2. **! 부정 논리 연산자를 두 번 사용하는 방법**

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
Boolean("x"); // true

// 2. ! 부정 논리 연산자를 두 번 사용하는 방법
!!"x"; // true
!!""; // false
```

## 단축 평가

### 논리 연산자를 사용한 단축 평가

논리합(\|\|) 또는 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수 있다. 논리합 또는 논리곱 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

```javascript
"Cat" && "Dog"; // "Dog"
```

논리곱(&&) 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다. 논리곱 연산자는 좌항에서 우항으로 평가가 진행된다.

첫 번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가된다. 하지만 이 시점까지는 위 표현식을 평가할 수 없다. 두 번째 피연산자까지 평가해 보아야 위 표현식을 평가할 수 있다. 이때 논리곱 연산자는 **논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 'Dog'를 그대로 반환한다.**

```javascript
"Cat" || "Dog"; // "Cat"
```

논리합(\|\|) 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다.

논리곱(&&) 연산자와 논리합(\|\|) 연산자는 **이처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그래도 반환한다. 이를 단축 평가<sup>short-circuit evaluation</sup>라 한다. 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.**

### 옵셔널 체이닝 연산자

ES11(ECMAScript2020)에서 도입된 옵셔널 체이닝<sup>optional chaining</sup> 연산자 ?. 는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

### null 병합 연산자

ES11(ECMAScript2020)에서 도입된 null 병합<sup>nullish coalescing</sup> 연산자 ?? 는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

> **<span style='color: grey'>이웅모, "모던 자바스크립트 Deep Dive", 위키북스(2020), p108-123.</span>**
