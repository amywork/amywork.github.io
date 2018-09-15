---
layout: post
title: "Data Model"
author: "Amy"
---

# Data Model, Structure와 Class
- Structure, Class, Enum - 프로그램 코드 블럭의 기본 구조
- Class 및 Structure는 인스턴스로 만들어질 때 프로퍼티를 모두 초기화 해야 한다. 때문에 초기 상태를 지정하기 위한 initializer가 만들어진다.
- 필요한 상황에서 instance를 생성하여 사용한다.
- Class의 인스턴스는 메모리의 Heap영역에 저장되는 참조(Reference)타입이다. 인스턴스가 대입된 변수는 이 인스턴스의 주소를 담고 있으며, 인스턴스를 가리키는 포인터라고 할 수 있다.
- Structure는 Class와 달리 값(Value) 타입이다.
- Data는 주로 Struct로 만들고, UI같이 재활용하는 것들은 주로 Class로 만든다. 하지만 무엇을 Struct로 만들어야 하고 Class로 만들어야 하는지는 개발자의 재량에 달려있다.

# Initializer

> init의 구체적인 내용은 스위프트 도큐멘테이션을 참조할 수 있다. [Initializer Swift Document Official Resources](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html)

1. Setting Initial Values for Stored Properties
2. Customizing Initialization
3. Default Initializers
4. Initializer Delegation for Value Types : Designated Initializer, Convenience Initializer
5. Class Inheritance and Initialization
6. Failable Initializers
7. Required Initializers
8. Setting a Default Property Value with a Closure or Function

# Struct의 Memberwise Initializer
- **Struct**의 경우 모든 프로퍼티가 초기화 할수 있게 모든 멤버에 대한 **Memberwise Initializer**를 제공한다.
- 때문에, Structure를 설계할 떄 모두 초기화 해주지 않아도 기본적으로 Memberwise Initializer를 통한 초기화를 보장할 수 있다.
- 하지만 커스텀으로 Struct 내에 custom initializer를 만들었다면, initializer에 포함되지 않은 프로퍼티에 대해서는 optional이던 기타 방법으로 Default 값을 넣어주어야 한다.

# Custom Initializer

{% highlight swift %}
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}

let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius is 37.0
{% endhighlight %}

# Designated Initializer
- Designated initializers are the primary initializers for a class. A designated initializer fully initializes all properties introduced by that class and calls an appropriate superclass initializer to continue the initialization process up the superclass chain. [sources](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html)


# Convenience Initializer
- Convenience initializers are secondary, supporting initializers for a class. You can define a convenience initializer to call a designated initializer from the same class as the convenience initializer with some of the designated initializer’s parameters set to default values. You can also define a convenience initializer to create an instance of that class for a specific use case or input value type. [sources](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html)


## Rule 1
- A designated initializer must call a designated initializer from its immediate superclass.
- designated initializer는 상위 클래스의 designated initializer를 반드시 호출해야 한다.

## Rule 2
- A convenience initializer must call another initializer from the same class.
- convenience initializer는 동일 클래스 내의 다른 initializer를 호출한다.

## Rule 3
- A convenience initializer must ultimately call a designated initializer.
- convenience initializer는 궁극적으로 designated initializer를 호출한다.

## A simple way to remember
- Designated initializers must always delegate up. = Designated는 상하 관계
- Convenience initializers must always delegate across. = Convenience는 좌우 관계

## Safety Check
- A convenience initializer must delegate to another initializer before assigning a value to any property (including properties defined by the same class). If it doesn’t, the new value the convenience initializer assigns will be overwritten by its own class’s designated initializer.

{% highlight swift %}
convenience init(name: String) {
    // 자신의 designated initializer 먼저 호출
    self.init(frame: CGRect(x:0,y:0,width:10,height:10))
    // designated initializer 먼저 호출한뒤 SomeView의 프로퍼티 초기화
    self.name = name
}
{% endhighlight %}

# Required Initializer 
- 필수적으로 구현해야 하는 Initializer
- 상속받은 클래스는 모두 구현해야 한다.
- 예시: Custom View만들 때 required init?(coder aDecoder: NSCoder)

{% highlight swift %}
required init?(coder aDecoder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
}
{% endhighlight %}

# Class 상속시 초기화
- 부모 클래스로부터 상속받은 모든 저장 속성은 서브 클래스를 초기화할 때 부모 클래스의 초기 값을 할당 받아야 한다.
- 부모 클래스의 Designated initializers를 호출 해야 한다.
- Class는 Type Casting을 사용할수 있다. (상속을 받았기 때문)

# Sample Code

{% highlight swift %}
import UIKit
class SomeView: UIView {
	
	// SomeView의 프로퍼티
	var name:String
	    
	// SomeView의 부모 class가 요구하는 required initializer
	required init?(coder aDecoder: NSCoder) {
	    fatalError("init(coder:) has not been implemented")
	}
	    
	// designated initializer
	override init(frame: CGRect) {
	    // SomeView의 프로퍼티를 초기화하는 동시에,
	    self.name = ""
	    // 상속을 받았으므로 부모 클래스의 Designated initializer를 호출하며 frame 초기화
	    super.init(frame: frame)
	}
	    
	// convenience initializer: name 값을 받으면서 초기화 하기 위함
	convenience init(name: String) {
	    // 자신의 designated initializer 호출
	    self.init(frame: CGRect(x:0,y:0,width:10,height:10))
	    // SomeView의 프로퍼티 초기화
	    self.name = name
	}

}
{% endhighlight %}

# Mutating func
- Value Type (Struct)에서의 프로퍼티 수정
- 기본적으로 구조체와 열거형과 같은 **값타입 프로퍼티**는 **인스턴스 메소드 내에서 바로 수정이 불가능**하다.- 때문에, 특정 메소드에서 수정을 해야 할 경우에는 mutating 키워드를 통해 변경 가능하다.

# Deinit
- Class instance에 대한 reference를 해제할 때 필요한 내용을 구현한다.

# Failable Initializers
- init이 실패했을 경우에 대한 처리가 가능하다.
- init뒤에 ?를 붙여 사용한다. (init?)
- init?(){ return nil }안에서 사용되는 return 값은 실제 값을 리턴한다기 보다 init이 실패하였다는 표기에 가깝다.
- 도큐멘트 원문: *Strictly speaking, initializers do not return a value. Rather, their role is to ensure that self is fully and correctly initialized by the time that initialization ends. Although you write return nil to trigger an initialization failure, you do not use the return keyword to indicate initialization success.*

{% highlight swift %}
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}
{% endhighlight %}
