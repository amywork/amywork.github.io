---
layout: post
title: "스탠포드 iOS 강의노트 v.03"
author: "younari"
---

## Lecture 5
### Lecture 5: Gestures

> **Course Description** Updated for iOS 10 and Swift. Tools and APIs required to build applications for the iPhone and iPad platforms using the iOS SDK. User interface design for mobile devices and unique user interactions using multi-touch technologies. Object-oriented design using model-view-controller paradigm, memory management, Swift programming language. Other topics include: object-oriented database API, animation, mobile device power management, multi-threading, networking and performance considerations.

- This course material is only available in the iTunes U app on iPhone or iPad.
- [Developing iOS 10 Apps with Swift
by Stanford](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120)

# Gestures
- Gestures are recognized by instances of **UIGestureRecognizer**
- Adding a gesture recognizer to a UIView

## UIPinchGestureRecognizer
- `var scale: CGFloat` // not read-only (can reset) 
- `var velocity: CGFloat { get }` // scale factor per second

{% highlight swift %}
// Pinch - UIPinchGestureRecognizer
@objc func changeScale(byReactingTo pinchRecognizer: UIPinchGestureRecognizer)
{
    switch pinchRecognizer.state {
    case .changed, .ended:
        scale *= pinchRecognizer.scale
        pinchRecognizer.scale = 1
    default:
        break
    }
}
{% endhighlight %}


## UIRotationGestureRecognizer
- `var rotation: CGFloat` // not read-only (can reset); in radians 
- `var velocity: CGFloat { get }` // radians per second## UISwipeGestureRecognizer
- `var direction: UISwipeGestureRecoginzerDirection` // which swipe directions you want 
- `var numberOfTouchesRequired: Int` // finger count

## UITapGestureRecognizer
- `var numberOfTapsRequired: Int` // single tap, double tap, etc. var 
- `numberOfTouchesRequired: Int` // finger count

{% highlight swift %}
@objc func toggleEyes(byReactingTo tapRecognizer: UITapGestureRecognizer)
{
    if tapRecognizer.state == .ended {
        let eyes: FacialExpression.Eyes = (expression.eyes == .closed) ? .open : .closed
        expression = FacialExpression(eyes: eyes, mouth: expression.mouth)
    }
}
{% endhighlight %}

## Sample Code
{% highlight swift %}
@IBOutlet weak var skullView: SkullView! {
    didSet {
        let handler = #selector(skullView.changeScale(byReactingTo:))
        
        let pinchRecognizer = UIPinchGestureRecognizer(target: skullView, action: handler)
        skullView.addGestureRecognizer(pinchRecognizer)
        
        let tapRecognizer = UITapGestureRecognizer(target: self, action: #selector(self.toggleEyes(byReactingTo:)))
        skullView.addGestureRecognizer(tapRecognizer)
        
        let swipeUpRecognizer = UISwipeGestureRecognizer(target:self, action: #selector(increaseHappiness))
        swipeUpRecognizer.direction = .up
        skullView.addGestureRecognizer(swipeUpRecognizer)
        
        let swipeDownRecognizer = UISwipeGestureRecognizer(target:self, action: #selector(decreaseHappiness))
        swipeDownRecognizer.direction = .down
        skullView.addGestureRecognizer(swipeDownRecognizer)
        
        updateUI()
    }
}
{% endhighlight %}