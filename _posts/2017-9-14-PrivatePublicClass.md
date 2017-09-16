---
layout: post
title: "Swift 클래스 접근 제어"
author: "younari"
---

# 01. Module
- **모듈** : 배포할 코드의 묶음 단위, 통상 프레임워크, 라이브러리, 애플리케이션

# 02. Source File
- **단일 swift 파일** : 하나의 스위프트 소스코드 파일


# 03. 접근 제어
- **캡슐화, 은닉화**를 위해 사용
- **Open** (개방 접근수준) : 모듈 외부까지 접근 가능
- **public** (공개 접근수준) : 모듈 외부까지 접근 가능
- **internal** (내부 접근수준) : 모듈 내부에서 접근가능, **기본 지정값**
- **fileprivate** (파일외 비공개) : 파일 내부에서만 접근가능
- **private** (비공개) : 기능 정의 내부에서만 가능 (단일 클래스 내부에서)

# 04. Open과 Public의 차이점
- 클래스의 상속 및 재정의(Override) 사용 가능 여부

- Open을 제외한 다른 모든 접근 수준의 클래스는 그 클래스가 정의된 모듈 안에서만 상속될 수 있다. 즉 클래스를 Open으로 명시하는 것은 그 클래스를 다른 모듈에서도 부모 클래스로 사용할수 있다는 뜻이다.
- Open을 제외한 다른 모든 접근 수준의 클래스 멤버는 그 멤버가 정의된 모듈 안에서만 재정의 될 수 있다. Open 수준의 클래스는 그 클래스가 정의된 모듈 밖의 다른 모듈에서도 상속되고, 재정의 될 수 있다.



{% highlight swift %}
public class SomePublicClass {     public var somePublicProperty = 0     var someInternalProperty = 0     fileprivate func someFilePrivateMethod() {}     private func somePrivateMethod() {}}// Internal 지정자가 Defaultclass SomeInternalClass {     var someInternalProperty = 0     fileprivate func someFilePrivateMethod() {}     private func somePrivateMethod() {}}

fileprivate class SomeFilePrivateClass {     func someFilePrivateMethod() {}     private func somePrivateMethod() {}}private class SomePrivateClass {     func somePrivateMethod() {}
}

{% endhighlight %}