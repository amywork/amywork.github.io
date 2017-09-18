---
layout: post
title: "Property 알아보기"
author: "younari"
---

# Swift Terminology - Property
##### Sources from [Glossary](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/GlossaryDefinitions.html#//apple_ref/doc/uid/TP40015214-CH12-SW1)

# Property
- 클래스, 구조체, 열거형등에서 사용되는 변수

# Stored Properties
- 클래스, 구조체에서만 인스턴스와 연관된 값을 저장

## Lazy property
- **lazy stored property**: 지연 저장 속성, 초기화 하는데 오래걸리거나 복잡한 초기화 과정이 있는 변수의 경우 지연저장을 사용하면 좋다.
- A lazy property does not get initialized until someone accesses itYou can allocate an object, execute a closure, or call a method if you want

# Computed Properties
- 클래스, 구조체, 열거형에서 사용할수 있는 프로퍼티
- 실제로 값을 저장하지 않지만, get, set키워드를 통해서 값을 간접적으로 설정하거나 받을 수 있다.

### Sample Code
{% highlight swift %}
struct Point {     var x = 0.0, y = 0.0 }struct Size {     var width = 0.0, height = 0.0 }struct Rect {var origin = Point() var size = Size() var center: Point {         get {             let centerX = origin.x + (size.width / 2)             let centerY = origin.y + (size.height / 2)             return Point(x: centerX, y: centerY)         }         set(newCenter) {             origin.x = newCenter.x - (size.width / 2)             origin.y = newCenter.y - (size.height / 2)         }} }
{% endhighlight %}


# Property Oberver
- **willSet** is called just before the value is stored.
- **didSet** is called immediately after the new value is stored.

### Sample Code
{% highlight swift %}
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
{% endhighlight %}


var list:[Int] = [] {
	didset
}


# Type Property
- 타입 자체에 대한 속성
- 값을 가져올때는 **클래스의 이름**을 통해서 가져올 수 있다.
- `let color: UIColor = UIColor.red`
- Instance properties are properties that belong to an instance of a particular type. Every time you create a new instance of that type, it has its own set of property values, separate from any other instance.
- You can also define **properties that belong to the type itself,** not to any one instance of that type. There will only ever be one copy of these properties, no matter how many instances of that type you create. These kinds of properties are called **type properties.**


### 👍🏻 Both types and instances can have methods & properties- Type methods and properties are denoted with the keyword **static.**- For example, the struct Double has a number of vars and funcs on its type. These are not methods or vars you access on an instance of a Double (e.g. on 53.2). Instead, **you access them by referencing the Double type itself.**


### 👍🏻 Sample Code

{% highlight swift %}
staticfuncabs(d:Double) -> Double {
	if d < 0 { 
		return -d
	}else{
		return d
	}
} 
{% endhighlight %}

- `static var pi: Double`- `let d = Double.pi`- `let d = Double.abs(-324.44)`- `let x: Double = 23.85`- `let e = x.pi` // no! pi is not an instance var- `let e = x.abs(-22.5)` // no! abs is not an instance method<hr>

# Memory
- **Code, Data, Heap, Stack**
- Class는 Code 영역에 저장
- Static Type Property는 Data 영역에 저장
- Instance의 속성들은 Heap 영역에 저장

# Value VS. Reference
### Value (struct and enum)- Copied when passed as an argument to a function- Copied when assigned to a different variable- Immutable if assigned to a variable with let (function parameters are let)- You must note any func that can mutate a struct/enum with the keyword mutating
### Reference (class)- Stored in the heap and reference counted (automatically)- Constant pointers to a class (let) still can mutate by calling methods and changing properties When passed as an argument, does not make a copy (just passing a pointer to same instance)