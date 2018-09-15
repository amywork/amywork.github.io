---
layout: post
title: "스탠포드 iOS 강의노트 L4"
author: "Amy"
---

## Lecture 4
### Lecture 4: Views

> **Course Description** Updated for iOS 10 and Swift. Tools and APIs required to build applications for the iPhone and iPad platforms using the iOS SDK. User interface design for mobile devices and unique user interactions using multi-touch technologies. Object-oriented design using model-view-controller paradigm, memory management, Swift programming language. Other topics include: object-oriented database API, animation, mobile device power management, multi-threading, networking and performance considerations.

- This course material is only available in the iTunes U app on iPhone or iPad.
- [Developing iOS 10 Apps with Swift
by Stanford](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120)


# Views
## Coordinate System Data Structures
- CGFloat
- CGPoint(x: _ , y: _)
- CGSize(width: _ , height: _)
- CGRect()
- `contains(CGRect)`
- `var minX: CGFloat`
- `var midY: CGFloat`
- `intersects(CGRect) -> Bool`
- `intersect(CGRect)`
- `contains(CGPoint)->Bool`
- Completely hiding a view without removing it from hierarchy
- `var hidden: Bool`

## UIColor
- Colors are set using UIColor.
- If you want to draw in your view with transparency...
- `var opaque = false`
- `UIColor.yellow.withAlphaComponent(0.5)`
- Colors can have alpha (transparency)
- `var alpha: CGFloat`

## Pixels and Points
- Pixels are the minimum-sized unit of drawing your device is capable of.
- Points are the units in the coordinate system.
- How many pixels per point are there?
- UIView's `var contentScaleFactor: CGFloat`
- `var bounds: CGRect` // a view's **internal drawing space's origin and size.**

## Fonts
- The absolutely best way to get a font in code
- Get preferred font for a given text style (e.g. body, etc.) using this UIFont type method
- `title?.font = UIFont.preferredFont(forTextStyle: .headline)`

## UIBezierPath
- `let path = UIBezierPath()`
- `path.move(to: CGPoint(80, 50))`
- `path.addLine(to: CGPoint(140, 150))`
- `path.close()`
- `let roundRect = UIBezierPath(roundedRect: CGRect, cornerRadius: CGFloat)`
- `let oval = UIBezierPath(ovalIn: CGRect)`
- Clipping your drawing to a UIBezierPath’s path `addClip()`


## Sample Code
{% highlight swift %}

import UIKit
@IBDesignable 
class SkullView: UIView {

    @IBInspectable
    var scale: CGFloat = 0.8
    @IBInspectable
    var eyesOpen: Bool = false
    @IBInspectable
    var mouthCurvature: Double = 1.0 // 1.0 is full smile, -1.0 is full frown
    @IBInspectable
    var lineWidth: CGFloat = 5.0
    
    // 원의 반지름
    private var skullRadius: CGFloat {
        return min(bounds.size.width, bounds.size.height) / 2 * scale
    }
    
    // 원의 중심
    private var skullCenter: CGPoint {
        return CGPoint(x: bounds.midX, y: bounds.midY)
    }
    
    // 얼굴의 골격 path
    private func pathForSkull() -> UIBezierPath {
        let path = UIBezierPath(arcCenter: skullCenter, radius: skullRadius, startAngle: 0, endAngle: CGFloat.pi * 2, clockwise: false)
        path.lineWidth = lineWidth
        return path
    }
    
    // 눈
    private enum Eye {
        case left
        case right
    }
    
    // 눈 path
    private func pathforEye(_ eye: Eye) -> UIBezierPath {
        
        func centerOfEye(_ eye: Eye) -> CGPoint {
            let eyeOffset = skullRadius / Ratios.skullRadiusToEyeOffset
            var eyeCenter = skullCenter
            eyeCenter.y -= eyeOffset
            eyeCenter.x += ((eye == .left) ? -1 : 1) * eyeOffset
            return eyeCenter
        }
        
        let eyeRadius = skullRadius / Ratios.skullRadiusToEyeRadius
        let eyeCenter = centerOfEye(eye)
        
        let path: UIBezierPath
        
        if eyesOpen {
            path = UIBezierPath(arcCenter: eyeCenter, radius: eyeRadius, startAngle: 0, endAngle:  CGFloat.pi * 2, clockwise: true)
        }else {
            path = UIBezierPath()
            path.move(to: CGPoint(x: eyeCenter.x - eyeRadius, y: eyeCenter.y))
            path.addLine(to: CGPoint(x: eyeCenter.x + eyeRadius, y: eyeCenter.y))
        }
        path.lineWidth = lineWidth
        return path
    }
    
    // 입 path
    private func pathforMouth() -> UIBezierPath {
        let mouthWidth = skullRadius / Ratios.skullRadiusToMouthWidth
        let mouthHeight = skullRadius / Ratios.skullRadiusToMouthHeight
        let mouthOffset = skullRadius / Ratios.skullRadiusToMouthOffset
        let mouthRect = CGRect(
            x: skullCenter.x - (mouthWidth / 2),
            y: skullCenter.y + mouthOffset,
            width: mouthWidth,
            height: mouthHeight)
        
        let start = CGPoint(x: mouthRect.minX, y: mouthRect.midY)
        let end = CGPoint(x: mouthRect.maxX, y: mouthRect.midY)
        let smileOffset = CGFloat(max(-1, min(mouthCurvature, 1))) * mouthRect.height
        let controlPoint1 = CGPoint(x: start.x + mouthRect.width/3, y: start.y + smileOffset)
        let controlPoint2 = CGPoint(x: end.x - mouthRect.width/3, y: start.y + smileOffset)

        let path: UIBezierPath = UIBezierPath(rect: mouthRect)
        path.move(to: start)
        path.addCurve(to: end, controlPoint1: controlPoint1, controlPoint2: controlPoint2)
        return path
    }
    
    
    // 자주쓰는 거리
    private struct Ratios {
        static let skullRadiusToEyeOffset: CGFloat = 3
        static let skullRadiusToEyeRadius: CGFloat = 20
        static let skullRadiusToMouthWidth: CGFloat = 1
        static let skullRadiusToMouthHeight: CGFloat = 3
        static let skullRadiusToMouthOffset: CGFloat = 5
    }
    
    // draw()
    override func draw(_ rect: CGRect) {
        UIColor.blue.set()
        pathForSkull().stroke()
        pathforEye(.left).stroke()
        pathforEye(.right).stroke()
        pathforMouth().stroke()
    }

}

{% endhighlight %}



