---
layout: post
title: "iOS User Interface Guide"
author: "younari"
---

# 🦄 iOS User Interface

## 00. Point, Pixel
- Xcode에서 개발 시 사용하는 단위는 모두 **point**이다. 실제 디자이너에게 전달 받을 이미지는 포인트 기준으로 1x, 2x, 3x **pixel**로 받아야 한다.

## 01. Frame Base
- 좌측 상단 0,0 을 기준으로, x,y 좌표를 산정하는 것
- **x, y, width, Height**
- 뷰의 위치는 **상위뷰**를 기준으로 위치를 잡는다. (상위 Layer 기준)

## 02. Auto Layout
- 추후 설명 추가 예정

# 🦄 Framework
- 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현 하는 **클래스와 라이브러리 모임**이다. 

# 🦄 UIKit framework
- Command + Shift + 0 으로 찾기
- **Cocoa Touch Framework**
- **Ex)** `import UIKit`
- **UI Kit** : Cocoa Touch Framework에 추가된 UI관련 기능의 클래스가 모여있는 Framework
- **UI Class Hierarchy** : NSObject 👉🏻 UIResponder 👉🏻 UIApplication, UIViewController, UIView 👉🏻 UIImageView, UILabel, UIControl, UIWindow, UIScrollView 👉🏻 UIButton, UISlider, UISwitch, UITextField


## UIResponder
- The UIResponder class defines an interface for objects that respond to handle events.
- ex) `open func becomeFirstResponder() -> Bool`

## UIApplication
## UIViewController
## UIView
- An object that manages the content for a rectangular area on the screen.

{% highlight swift %}
let topView: UIView = UIView(frame: CGRect(x: 15, y: 15, width: self.view.frame.width-30, height: 100))
{% endhighlight %}

- UIView class의 property 중의 하나인, frame(=> CGRect 타입) 에 값을 넣어서 init하는 방식

#### - CGRect?
- **struct CGRect**: A structure that contains the location and dimensions of a rectangle. (CG = Core Graphic)
- In the default **Core Graphics** coordinate space, the origin is located in the lower-left corner of the rectangle and the rectangle extends towards the upper-right corner. If the context has a flipped-coordinate space—often the case on iOS—the origin is in the upper-left corner and the rectangle extends towards the lower-right corner.

{% highlight swift %}
let topView: UIView = UIView(frame: CGRect(x: 15, y: 15, width: self.view.frame.width-30, height: 100))

topView.frame = CGRect(origin: CGPoint(x: 30, y: 30), size: CGSize(width: 100, height: 100))
topView.backgroundColor = UIColor.yellow
topView.layer.borderWidth = 8
topView.layer.borderColor = UIColor.black.cgColor
//UIColor라는 class 안에 black이라는 프로퍼티가 있고, cgColor로 변환
topView.layer.cornerRadius = 20
self.view.addSubview(topView)
{% endhighlight %}

## UILabel
- A view that displays one or more lines of read-only text, often used in conjunction with controls to describe their intended purpose.

## UIImageView
- An object that displays a single image or a sequence of animated images in your interface.


## UIControl
## UIWindow
## UIScrollView

