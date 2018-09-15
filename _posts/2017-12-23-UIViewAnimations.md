---
layout: post
title: "Play with Animation"
author: "Amy"
---

> Swift로 애니메이션 만들어보기

# WWDC
- [Advances in UIKit Animations and Transitions](https://developer.apple.com/videos/play/wwdc2016/216/)

# Official Documents
- [UIViewPropertyAnimator / class](https://developer.apple.com/documentation/uikit/uiviewpropertyanimator) :: A class that animates changes to views and allows the dynamic modification of those animations.
- [UIViewAnimating / Protocol](https://developer.apple.com/documentation/uikit/uiviewanimating) :: An interface for implementing custom animator objects.

# RayWenderlich
- [iOS Animations by Tutorials](https://store.raywenderlich.com/products/ios-animations-by-tutorials)


# Sample code

## 01. Label Cube Animation
- 라벨의 텍스트를 Happy -> NewYear 로 페이지 넘기듯 전환하기

{% highlight swift %}
class ViewController: UIViewController {

    @IBOutlet weak var helloLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        helloLabel.backgroundColor = .clear
        delay(seconds: 3.0) {
            self.cubeAnimate(label: self.helloLabel, text: "NEW YEAR!")
        }
        
    }
    
    func cubeAnimate(
        label: UILabel,
        text: String
    ) {
        
        let tempLabel = duplicateLabel(label: label)
        tempLabel.text = text
        
        let tempLabelOffset = label.frame.size.height/2.0
        
        let scale = CGAffineTransform(scaleX: 1.0, y: 0.1)
        let translate = CGAffineTransform(translationX: 0.0, y: tempLabelOffset)
        
        tempLabel.transform = translate.concatenating(scale)
        view.addSubview(tempLabel)
        
        UIView.animate(
            withDuration: 0.6,
            animations: {
                tempLabel.transform = .identity
                label.transform = translate.concatenating(scale)
        }) { _ in
            label.text = tempLabel.text
            label.transform = .identity
            tempLabel.removeFromSuperview()
        }
    }
    
    func duplicateLabel(label: UILabel) -> UILabel {
        let newLabel = UILabel(frame: label.frame)
        newLabel.font = label.font
        newLabel.textAlignment = label.textAlignment
        newLabel.textColor = label.textColor
        newLabel.backgroundColor = label.backgroundColor
        return newLabel
    }
    
    func delay(seconds: Double, completion: @escaping ()-> Void) {
        DispatchQueue.main.asyncAfter(deadline: .now() + seconds, execute: completion)
    }
    
}
{% endhighlight %}


## 02. Key frames

- `UIView.animateKeyframes`
- `UIView.addKeyframe`
- `withRelativeStartTime`
- `relativeDuration`


{% highlight swift %}

UIView.animateKeyframes(
    withDuration: 1.5,
    delay: 0.0,
    animations: {
        UIView.addKeyframe(withRelativeStartTime: 0.0, relativeDuration: 0.25, animations: {
            self.planeImage.center.x += 80.0
            self.planeImage.center.y -= 10.0
        })
        
        UIView.addKeyframe(withRelativeStartTime: 0.1, relativeDuration: 0.4, animations: {
            self.planeImage.transform = CGAffineTransform(rotationAngle: -.pi/5)
        })
        
        UIView.addKeyframe(withRelativeStartTime: 0.25, relativeDuration: 0.25, animations: {
            self.planeImage.center.x += 100
            self.planeImage.center.y -= 50.0
            self.planeImage.alpha = 0.0
        })
        
        UIView.addKeyframe(withRelativeStartTime: 0.51, relativeDuration: 0.01, animations: {
            self.planeImage.transform = .identity
            self.planeImage.center = CGPoint(x: 0.0, y: originalCenter.y)
        })
        
        UIView.addKeyframe(withRelativeStartTime: 0.55, relativeDuration: 0.45, animations: {
            self.planeImage.center = originalCenter
            self.planeImage.alpha = 1.0
        })
        
},
    completion: nil)
    
{% endhighlight %}



## 03. ViewController Transition

- `UIViewControllerAnimatedTransitioning` 프로토콜을 채택한,
- 커스텀 PopAnimator transition class를 만든다. 
- `transitionDuration`과 `animateTransition` 메소드를 구현한다.

{% highlight swift %}

class PopAnimator: NSObject, UIViewControllerAnimatedTransitioning {
	let duration = 1.0
	var originFrame = CGRect.zero
	var presenting = true
}

{% endhighlight %}

{% highlight swift %}

// Duration
func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval {
    return duration
}

func animateTransition(using transitionContext: UIViewControllerContextTransitioning) {
	// Customize transition effects
}

{% endhighlight %}

- 대상 viewController는 `UIViewControllerTransitioningDelegate`를 채택하고 아래와 같이 작업한다. 

{% highlight swift %}
// 커스텀하게 만든 transition의 인스턴스를 생성한다.
let transition = PopAnimator()

// delegate 메소드를 정의한다.
func animationController(
    forPresented presented: UIViewController,
    presenting: UIViewController,
    source: UIViewController
    ) -> UIViewControllerAnimatedTransitioning? {
    return transition
}
    
func animationController(forDismissed dismissed: UIViewController) -> UIViewControllerAnimatedTransitioning? {
    return transition
}
{% endhighlight %}
