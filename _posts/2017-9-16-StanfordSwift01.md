---
layout: post
title: "스탠포드 iOS 강의노트 v.01"
author: "younari"
---


## Intro to iOS 10, Xcode 8, Swift 3 - Note v0.1

> **Course Description** Updated for iOS 10 and Swift. Tools and APIs required to build applications for the iPhone and iPad platforms using the iOS SDK. User interface design for mobile devices and unique user interactions using multi-touch technologies. Object-oriented design using model-view-controller paradigm, memory management, Swift programming language. Other topics include: object-oriented database API, animation, mobile device power management, multi-threading, networking and performance considerations.

- This course material is only available in the iTunes U app on iPhone or iPad.
- [Developing iOS 10 Apps with Swift
by Stanford](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120)

### Mandatory Articles
- [Swift Programming Language](https://developer.apple.com/swift/)
- [Visit Apple's iOS Dev Center](https://developer.apple.com/)



### Class: UIViewController
- UIViewController controls UI
- **Outlet** = Instances and Variables (Properties)
- `@IBOutlet weak var display: UILabel? = nil`
- **Action** = Method
- `@IBAction func touchDigit(_ sender: UIButton) {}`


### Arguments and Parameters
- **External Name**: Arguments when we call a function
- **Internal Name**: Parameters when we define a function

<br>
<hr>
<br>

## DIY. Make calculator
- **Optional**: Set / Not set
- **nil**: optional, not set


### 01. User Typing Digits

{% highlight swift %}

var isUserTyping: Bool = false
    
@IBAction func touchDigit(_ sender: UIButton) {
    let digit = sender.currentTitle!
    
    if isUserTyping {
        let textCurrentlyInDisplay = display!.text!
        display!.text = textCurrentlyInDisplay + digit
    }else {
        display!.text = digit
        isUserTyping = true
    }
}

{% endhighlight %}


### 02. Computed Properties
- read only variables
- get some values from~ and set some values to~
- Double to String


{% highlight swift %}

// Computed properties
var displayValue: Double {
    get {
        return Double(display!.text!)!
    }
    set {
        display?.text = String(newValue)
    }
}
{% endhighlight %}


### 03. Define Operating Action
{% highlight swift %}

@IBAction func operation(_ sender: UIButton) {
    isUserTyping = false
    if let mathSymbol = sender.currentTitle {
        switch mathSymbol {
        case "√" :
            displayValue = sqrt(displayValue)
        default :
            break
        }
    }
}
{% endhighlight %}


### 04. Divide Operation to Model
- 계산 기능을 Model(API)로 만들기
- 03번의 계산 기능을 UI에서 분리하는 작업

{% highlight swift %}
import Foundation
//  This is the Model of Calculator Brain.
//  UI Independent, Read - only.

struct CalculatorBrain {
    
    private var accumulator: Double
    
    func performOperation(_ symbol: String) {
  
    }
    
    func setOperand(_ operand: Double) {
      
    }
    
    // Computed properties
    var result: Double {
        get {
            
        }
    }
}
{% endhighlight %}


### 05. Operation Action과 Model 연결

{% highlight swift %}

private var brain: CalculatorBrain = CalculatorBrain()

@IBAction func performOperation(_ sender: UIButton) {
    if isUserTyping {
        brain.setOperand(displayValue)
        isUserTyping = false
    }
    
    if let mathSymbol = sender.currentTitle {
        brain.performOperation(mathSymbol)
    }
    
    if let result = brain.result {
        displayValue = result
    }
}
    
{% endhighlight %}


### 06. Model 기능 완성하기
- 계산기의 Operation Model을 완성하는 2화 45분 ~ 는 현재 Swift 문법을 배우는 과정에서 완벽하게 이해하기 쉽지 않으므로 추후에 다시 Back할 예정입니다.