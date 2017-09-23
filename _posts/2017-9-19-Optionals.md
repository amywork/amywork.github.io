---
layout: post
title: "스위프트 옵셔널 문법"
author: "younari"
---

# iOS and Swift Terminology - Optional
##### Sources from [developer.apple.com](https://developer.apple.com/documentation/swift/optional)

> A type that represents either a wrapped value or nil, the absence of a value

## Optional?
- A value that contains either an underlying value or nil to indicate that the value is missing.

### 😱 optional도 enum이다.
- The Optional type is an enumeration with two cases. 
- **Optional.none** is equivalent to the nil literal. 
- **Optional.some(Wrapped)** stores a wrapped value. 
- `case some(Wrapped)`
- `case none`

{% highlight swift %}
let number: Int? = Optional.some(42)
let noNumber: Int? = Optional.none
{% endhighlight %}

{% highlight swift %}
let x: String? = ...
if let y = x { //do something with y }
{% endhighlight %}

- 위 코드와 아래 코드와 같은 의미를 갖고 있다.

{% highlight swift %}
swithch x {
	case .some(let y): // do something with y
	case .none: break 
}
{% endhighlight %}

## nil
- 변수만 선언되어 있으며, 아직 instance가 할당되기 전
- 아무것도 없는 상태

## Unwrap
- Optional 변수에 값이 있음을 확인하여 일반 변수로 전환해준다.
- nil인 상태에서 속성을 참조하거나, 함수를 실행시 발생하는 크래쉬를 방지하기 위해 사용
- To extract an underlying value from an optional.

### 🤡 Forced Unwrapping : 강제 해제 
- 값이 있다고 내가(개발자) 보장하니, 강제 해제를 실행하라!

{% highlight swift %}
func testFuc(optionalStr:String?){
	if optionalStr != nil { // 크래쉬 방지		let unwrapStr:String = optionalStr! 
		print(unwrapStr)	}
}
{% endhighlight %}

 
### 🤡 Optional Binding : if let
- unwrap할 값이 있으면, if 문이 실행되고
- unwrap할 값이 없으면, if 문이 무시된다.
- The process of attempting to assign an optional value to a constant in a conditional statement to see if the optional contains an underlying value.

{% highlight swift %}
func testFuc(optionalStr:String?) {	if let unwrapStr = optionalStr {       print(unwrapStr)    }}
{% endhighlight %}

### 🤡 Early Exit : guard문

{% highlight swift %}
guard 조건값 else {		//조건값이 거짓일때 실행}
{% endhighlight %}

{% highlight swift %}
func getFriendList(list:[String]?) {	guard let list = list else { return }
	// list가 nil이 아닐 때에만 아래를 실행하라
	for name in list {
		if name == "abcd" {
			print("find")    	}
    }
 }
{% endhighlight %}

## Optional Chaining
- **nil 이면 더이상 뒤로 실행하지 말아줘.**
- [Optional Chaining - Swift Guide](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html#//apple_ref/doc/uid/TP40014097-CH21-ID245)

{% highlight swift %}
if let isPNG = imagePaths["star"]?.hasSuffix(".png") {
    print("The star image is in PNG format")
}
{% endhighlight %}

- 아래 두 코드를 비교해보면 위의 x는 Int 이고, 아래 x는 Int?이다.
- `if let x = display?.text?.hashValue { ... }`
- `let x = display?.text?.hashValue { ... }`

## Using the Nil-Coalescing Operator
- **nil 이면 디폴트값을 실행해줘.**
- Use the nil-coalescing operator (??) to supply a default value in case the Optional instance is nil. Here a default path is supplied for an image that is missing from imagePaths.

{% highlight swift %}
let defaultImagePath = "/images/default.png"
let heartPath = imagePaths["heart"] ?? defaultImagePath
print(heartPath)
// Prints "/images/default.png"
{% endhighlight %}