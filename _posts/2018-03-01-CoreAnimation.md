---
layout: post
title: "Core Animation API"
author: "Amy"
---

# Resources
- [GIT_LayerPlayer](https://github.com/raywenderlich/LayerPlayer)
- [GIT_3DTransformFun](https://github.com/jrturton/3DTransformFun)

# CALayer
- UIView는 CALayer의 Wrapper이다.
- UIView에 Bounds를 set하는 행위는 곧 뷰를 backing하고 있는 CALayer에 Bounds를 set하는 것과 같다.
- UIView에 `layoutIfNeeded`를 부르면, 해당 뷰의 `root CALayer`로 전달된다.
- 모든 UIView는 하나의 root CALayer를 갖고 있으며, 요놈이 sublayer들을 배열로 가지고 있다.

<hr>

### Layer는 SubLayer를 가질 수 있다.
### Layer는 여러가지 Property가 있다.
### Layer의 Property들은 animate될 수 있다.
### Layer는 View에 비해 퍼포먼스가 좋다.

<hr>

# shadow
- [How Shadow Works](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_shadows/dq_shadows.html#//apple_ref/doc/uid/TP30001066-CH208-SW1)

```
let layer = CALayer()
layer.frame = aView.bounds
layer.shadowOpacity = 0.8
layer.shadowOffset = CGSize(width: 2, height: 0)
layer.shadowRadius = 3.0
aView.layer.addSublayer(layer)
```

# contents

```
layer.contents = UIImage(named: "imgName")?.cgImage // Set Quartz image data (CGImage)
layer.contentsGravity = kCAGravityCenter
```

# color, opacity...
```
layer.backgroundColor = UIColor.red.cgColor
layer.opacity = 1.0
layer.isHidden = false
layer.masksToBounds = false
layer.cornerRadius = 100.0
layer.borderWidth = 12.0
layer.borderColor = UIColor.white.cgColor
```

# shouldRasterize
- `false` by default
- `true`로 바꾸면 퍼포먼스 향상에 좋으나, misuse 주의할 것
- ex. 모양은 바뀌지 않고 animate 될 때

# drawsAsynchronously
- `false` by default
- `true`로 바꾸면 퍼포먼스 향상에 좋으나, misuse 주의할 것
- ex. CAEmitterLayer에서 아이콘이 쓰일 때

# animate
- layer의 property를 변화시켜서 애니메이션 효과를 줄 수 있다.
- 출처: [RayWenderlich](https://www.raywenderlich.com/169004/calayer-tutorial-ios-getting-started)

```
@IBAction func pinchGestureRecognized(_ sender: UIPinchGestureRecognizer) {
  let offset: CGFloat = sender.scale < 1 ? 5.0 : -5.0
  let oldFrame = layer.frame
  let oldOrigin = oldFrame.origin
  let newOrigin = CGPoint(x: oldOrigin.x + offset, y: oldOrigin.y + offset)
  let newSize = CGSize(width: oldFrame.width + (offset * -2.0), height: oldFrame.height + (offset * -2.0))
  let newFrame = CGRect(origin: newOrigin, size: newSize)
  if newFrame.width >= 100.0 && newFrame.width <= 300.0 {
    layer.borderWidth -= offset
    layer.cornerRadius += (offset / 2.0)
    layer.frame = newFrame
  }
}
```


# CAScrollLayer
- 가볍게 코드로 스크롤을 해야할 경우에만 사용한다.
- 유저의 스크롤은 `UIScrollView` 사용 권장
- 큰 이미지를 스크롤 할 때는 `CATiledLayer` 사용 권장
- 출처: [RayWenderlich](https://www.raywenderlich.com/169004/calayer-tutorial-ios-getting-started)

```
@IBAction func panRecognized(_ sender: UIPanGestureRecognizer) {
  var newPoint = scrollingView.bounds.origin
  newPoint.x -= sender.translation(in: scrollingView).x
  newPoint.y -= sender.translation(in: scrollingView).y
  sender.setTranslation(CGPoint.zero, in: scrollingView)
  // 3
  scrollingViewLayer.scroll(to: newPoint)
  
  if sender.state == .ended {
    UIView.animate(withDuration: 0.3, delay: 0, options: [], animations: {
        self.scrollingViewLayer.scroll(to: CGPoint.zero)
    })
  }
}
```

# CATextLayer
```
let textLayer = CATextLayer()
textLayer.frame 
textLayer.string = string
textLayer.font = CTFontCreateWithName(fontName, fontSize, nil)
textLayer.foregroundColor = UIColor.darkGray.cgColor
textLayer.isWrapped = true
textLayer.alignmentMode = kCAAlignmentLeft
textLayer.contentsScale = UIScreen.main.scale
someView.layer.addSublayer(textLayer)
```

# AVPlayerLayer
- [필요할 때 다시보기](https://www.raywenderlich.com/169004/calayer-tutorial-ios-getting-started)

# CAGradientLayer
```
let gradientLayer = CAGradientLayer()
gradientLayer.frame = aView.bounds
gradientLayer.colors = [cgColor array]
gradientLayer.startPoint = CGPoint(x: 0, y: 0)
gradientLayer.endPoint = CGPoint(x: 0, y: 1)
aView.layer.addSublayer(gradientLayer)
```

# CAReplicatorLayer
- [필요할 때 다시보기](https://www.raywenderlich.com/169004/calayer-tutorial-ios-getting-started)

```
let replicatorLayer = CAReplicatorLayer()
replicatorLayer.frame = aView.bounds
replicatorLayer.instanceCount = 30
replicatorLayer.instanceDelay = CFTimeInterval(1 / 30.0)
replicatorLayer.preservesDepth = false
replicatorLayer.instanceColor = UIColor.white.cgColor
```

# CATiledLayer
- [필요할 때 다시보기](https://www.raywenderlich.com/169004/calayer-tutorial-ios-getting-started)
- levelsOfDetailBias
- levelsOfDetail
- Asynchronous drawing (퍼포먼스 향상)


# CAShapeLayer

```
let aLayer = CAShapeLayer()
aLayer.path = aPath.cgPath
aLayer.fillColor = aColor.cgColor
aLayer.fillRule = kCAFillRuleNonZero
aLayer.lineCap = kCALineCapButt
aLayer.lineDashPattern = nil
aLayer.lineDashPhase = 0.0
aLayer.lineJoin = kCALineJoinMiter
aLayer.lineWidth = 1.0
aLayer.miterLimit = 10.0
aLayer.strokeColor = aColor.cgColor
```

# CATransformLayer

# CAEmitterLayer

```
let emitterLayer = CAEmitterLayer()
let emitterCell = CAEmitterCell()
emitterLayer.frame = view.bounds
emitterLayer.seed = UInt32(Date().timeIntervalSince1970)
emitterLayer.renderMode = kCAEmitterLayerAdditive
emitterLayer.drawsAsynchronously = true
```