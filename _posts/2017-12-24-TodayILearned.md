---
layout: post
title: "Slide out effect"
author: "younari"
---

> 12월 마지막 주, 크리스마스를 스위프트와 함께... ✨ 오늘은 RayWenderlich의 [Slide out navigation panel]((https://www.raywenderlich.com/78568/create-slide-out-navigation-panel-swift)) 튜토리얼을 따라해보는 시간을 가졌습니다. 

# Slide Out Flow
- `ContainerViewController`가 window의 root가 된다.
- `ContainerViewController`가 property로 `centerViewController`, 
  `centerNavigationController`를 가지고 있다.
- `ContainerViewController`에서 아래 코드를 통해, `ContainerViewController.view`와 `centerNavigationController.view`의 위계를 만든다.

```
view.addSubview(centerNavigationController.view)
addChildViewController(centerNavigationController)
centerNavigationController.didMove(toParentViewController: self)
```

- `ContainerViewController`는 `CenterViewControllerDelegate` 프로토콜을 채택하고, `CenterViewControllerDelegate`의 delegate가 된다.

- `ContainerViewController`는 Slide State를 Enum으로 갖고 있다.

{% highlight swift %}
enum SlideState {
	case leftExpanded // 왼쪽 메뉴만 열린 경우
	case rightExpanded // 오른쪽 메뉴만 열린 경우
	case bothCollapsed // 모두 닫힌 경우 (default)
	
}
{% endhighlight %}

- 현재 `ContainerViewController`의 디폴트 Slide 상태는 `.bothCollapsed`이다.

```
var currentState: SlideOutState = .bothCollapsed
```

- `ContainerViewController`는 `leftViewController`를 프로퍼티로 갖고 있다.
- `ContainerViewController`에서 panel을 toggle 하는 데 관여하는 delegate 메소드를 모두 구현해준다.

<hr>

### `func toggleLeftPanel()`
- shouldExpand : Bool 값에 따라, animateLeftPanel을 콜한다.

{% highlight swift %}
func toggleLeftPanel() {
	let shouldExpand: Bool = currentState != .leftExpanded
	if shouldExpand {
	  addLeftPanelViewController()
	}
	animateLeftPanel(shouldExpand: shouldExpand)
}
{% endhighlight %}


### `func animateCenterPanelXPosition()`
- `targetPosition` 과 `completion` 핸들러를 받아, `Center X` 포지션을 바꿔주는 animate 메소드

```
func animateCenterPanelXPosition(
    targetPosition: CGFloat,
    completion: ((Bool)->Void)? = nil) {
    
    UIView.animate(withDuration: 1,
                   delay: 0,
                   usingSpringWithDamping: 0.8,
                   initialSpringVelocity: 0,
                   options: .curveEaseIn,
                   animations: {
                    self.centerNavigationController.view.frame.origin.x = targetPosition
    }, completion: completion)
}
```

### `func animateLeftPanel()`
- `shouldExpand` 값에 따라, `currentState`를 바꾸고, `animateCenterPanelXPosition` 메소드를 실행

```
// let centerOffset: CGFloat = 60
func animateLeftPanel(shouldExpand: Bool) {
	if shouldExpand {
	  currentState = .leftExpanded
	  animateCenterPanelXPosition(
	    targetPosition: centerViewController.view.frame.width - centerOffset
	  )
	}else {
	  animateCenterPanelXPosition(
	    targetPosition: 0,
	    completion: { (finished) in
	      self.currentState = .bothCollapsed
	      self.leftViewController?.view.removeFromSuperview()
	      self.leftViewController = nil
	  })
	}
}
```
