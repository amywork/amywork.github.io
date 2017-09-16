---
layout: post
title: "스탠포드 스위프트 강의노트 01"
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


### DIY. Make calculator
- **Optional**: Set / Not set
- **nil**: optional, not set


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



### Computed Properties
- read only variables
- get some values from~ and set some values to~

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
