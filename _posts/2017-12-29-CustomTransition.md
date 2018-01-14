---
layout: post
title: "Custom Transition"
author: "younari"
---

> Navigation Controller에 올라탄 view controller들의 transition 효과를 커스텀해본다.


# Navigation Controller Delegate
- `NavigationController`의 transition을 customize 하기 위해, `NavigationControllerDelegate` Class를 생성해서, 원하는 지점에 커스텀한 `UIViewControllerAnimatedTransitioning`클래스를 return 해준다.


```
import UIKit
class NavigationControllerDelegate: NSObject, UINavigationControllerDelegate {
    
    func navigationController(_ navigationController: UINavigationController, animationControllerFor operation: UINavigationControllerOperation, from fromVC: UIViewController, to toVC: UIViewController) -> UIViewControllerAnimatedTransitioning? {
        
        if fromVC.isKind(of: StartingController.self) {
            return ShowTransition()
        }else if fromVC.isKind(of: EndingController.self) {
            return DismissTransition()
        }else {
            return nil
        }
        
    }
}

```


# Custom Transition
- `UIViewControllerAnimatedTransitioning` 프로토콜을 채택한 커스텀 transition 클래스를 만든다.

```
class ShowTransition: NSObject, UIViewControllerAnimatedTransitioning 
class DismissTransition: NSObject, UIViewControllerAnimatedTransitioning 
```

- `UIViewControllerAnimatedTransitioning` 프로토콜이 강제하는 `animateTransition`, `transitionDuration` 메소드를 구현해준다.

```
func animateTransition(using transitionContext: UIViewControllerContextTransitioning)
func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval
```

- `animateTransition` 메소드 내에서, `containerView`를 따고, 여기에 요구사항에 맞는 적절한 애니메이션을 넣어준다.

```
// sample
guard
    let sourceViewController = transitionContext.viewController(forKey: .from),
    let destinationController = transitionContext.viewController(forKey: .to),
    let destinationView = destinationController.view,
    else { return }
    
let containerView = transitionContext.containerView
containerView.addSubview(destinationView)
```
