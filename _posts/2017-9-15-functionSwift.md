---
layout: post
title: "Swift 함수"
author: "younari"
---

> 함수는 클로저의 일부이다.

### 01. Argument Labels and Parameter Names
- **Argument**(인수레이블)은 함수 호출시 사용 되는 이름표
- **Parameter**(매개변수)는 함수 내부에서 사용 되는 변수명
- 인수 레이블은 생략 가능하며 없을 때는 매개변수명이 인수 레이블로 사용된다.

```swift
func fName(aName pName:Int) -> Int {      return paramName + 3  }
```

### 02. In-Out Parameter Keyword
- 파라메터에 **함수 외부의 인스턴스**가 직접 들어간다.
- 외부에 존재하는 변수를 함수를 통해 제어하고 싶을 때 사용한다.
- &기호: 포인터의 개념, 해당 변수의 메모리 주소값
- `var someInt = 3`
- `var anotherInt = 107`
- `swapTwoInts(&someInt, &anotherInt)`

{% highlight swift %}
func swapTwoInts(_ a: inout Int, _ b: inout Int) {	let temporaryA = a	a=b	b = temporaryA}
{% endhighlight %}


### 03. Return
- return 키워드를 사용하여 함수 결과 반환. - 반환 타입과 같은 타입의 데이터를 반환 해야 한다.
- 반환값이 없는 경우는 Retrun Type을 작성하지 않는다.
- return 타입에 tuple 타입을 넣을 수도 있다. 