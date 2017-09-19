---
layout: post
title: "Swift Optional Type"
author: "younari"
---

# iOS and Swift Terminology - Optional
##### Sources from [developer.apple.com](https://developer.apple.com/documentation/swift/optional)

## nil
- 변수만 선언되어 있으며, 아직 instance가 할당되기 전
- 아무것도 없는 상태

## optional
- A type that represents either a wrapped value or nil, the absence of a value.
- A value that contains either an underlying value or nil to indicate that the value is missing.
- nil인 상태에서 속성을 참조하거나, 함수를 실행시 발생하는 error를 방지하기 위해 사용한다.
- `var num: Int?` -> **Optional Int**

## unwrap
- Optional 변수에 값이 있음을 확인하여 일반 변수로 전환해준다.
- To extract an underlying value from an optional.

### Forced Unwrapping : 강제 해제 
- 값이 있다고 강제 해제를 하는 것

{% highlight swift %}
func testFuc(optionalStr:String?){
	if optionalStr != nil {		let unwrapStr:String = optionalStr! 
		print(unwrapStr)	}
}
{% endhighlight %}

 
### Optional Binding : if let
- optionalStr이 unwrap할것이 있으면, if문이 실행되고
- unwrap할 값이 없으면 if 문이 무시된다.
- The process of attempting to assign an optional value to a constant in a conditional statement to see if the optional contains an underlying value.

{% highlight swift %}
func testFuc(optionalStr:String?) {	if let unwrapStr = optionalStr {       print(unwrapStr)    }}
{% endhighlight %}

### Early Exit : guard문

{% highlight swift %}
guard 조건값 else {		//조건값이 거짓일때 실행}
{% endhighlight %}


func getFriendList(list:[String]?) {	guard let list = list else { return }
	// list가 nil이 아닐 때에만 아래를 실행하라
	for name in list {
		if name == "abcd" {
			print("find")    	}
    }
 }

