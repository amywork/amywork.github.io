---
layout: post
title: "Problem Solving"
author: "younari"
---

> 사수님께 배운 내용, 구글링한 내용 등 각종 문제 상황을 해결하면서 남은 지식들 🙂🙃

# 01. 뷰콘트롤러가 pop될 때 deinit이 불리는지 확인한다.
- 클로저에서 Self를 캡처하거나 delegate = self 에서 해당 뷰콘트롤러의 Retain count를 실수로 올릴 수 있다. weak 이나 guard `self`를 통해 메모리 관리에 주의해야 한다.
- **Delegate, Closure 에서 weak 챙겨주기**

<br>

# 02. Frame과 Bounds
- "너가 어디에 있든 관심이 없고, 너의 보여지는 중앙에 위치하고 싶을 때" -> Bounds! (대상 뷰의 바운드 mid)
- "나는 너의 종속된 좌표로 들어가고 싶어!" -> 뷰와 뷰의 구성에는 Frame!
- 콜렉션뷰 인셋을 주었을 때 바운드 값이 변한다 -> 바운드는 카메라의 위치라고 생각해보자.

<br>

# 03. Copy와 DeepCopy
- (objc) `DeepCopy`는 클래스 내부의 프로퍼티까지 메모리 주소를 싹 다 새로 만든다.

<br>

# 04. 스크롤 퍼포먼스
- **현상**: 핀터레스트 레이아웃으로, CollectionView에서 2단 그리드 형태로 Cell을 그려낼 때 스크롤 하면서 Cell의 움직임이 버벅이는 현상
- **원인**: 아이폰은 1초에 60프레임을 그린다. 다른 작업 때문에 Main Thread에서 초당 20프레임 이하로 떨어지면 UI가 버벅이는 현상이 발생한다. UI 작업 이외의 dataRequest 하는 부분은 concurrent로 돌리고, attributedString 등 연산이 들어가는 행위는 Cell마다 하지 않고 1번만 할 수 있도록 분리시킨다.
- **인사이트**: 메인 스레드에서 UI를 그리는 데 시간을 몰아주어야 한다. 연산은 1번만 한다. 

<br>

# 05. AutoLayout 적용 시점
- setNeedsLayout / layoutIfNeeded / layoutSubviews의 차이점은 뭘까?
- setNeedsLayout: asynchronous update (비동기 업데이트)
- layoutIfNeeded: synchronous update (동기 업데이트 = 바로 실행)
- layoutSubviews: override method
- (발췌) So, stated succinctly, layoutIfNeeded says update immediately please, whereas setNeedsLayout says please update but you can wait until the next update cycle.
- [참고자료01](http://www.iosinsight.com/setneedslayout-vs-layoutifneeded-explained/)
- [참고자료02](https://medium.com/@abhimuralidharan/ios-swift-setneedslayout-vs-layoutifneeded-vs-layoutsubviews-5a2b486da31c)

<br>

# 06. init rect & coder
- init(rect:)는 뷰를 코드로 생성할때 사용하는 방식
- init(coder:)는 뷰를 xib로 생성할때 사용하는 방식
- 예시로 아래에서는 스토리보드를 통해 init 했으므로, init(coder:)가 불림

```
collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "MyIdentifier", for: indexPath) 
```

<br>

# 07. Xcode config
- [Using xcconfig files](http://www.jontolof.com/cocoa/using-xcconfig-files-for-you-xcode-project/)

<br>

# 08. ScrollView
- `scrollRectToVisible(_:animated:)`
- [Apple Doc](https://developer.apple.com/documentation/uikit/uiscrollview/1619439-scrollrecttovisible#declarations)
- [stackoverflow](https://stackoverflow.com/questions/1446536/uiscrollview-works-as-expected-but-scrollrecttovisible-does-nothing)

<br>

# 09. UIColor Extension
- UIColor RGB로 컬러 만들기

```
extension UIColor {
    
    static func rgb(_ red: CGFloat, green: CGFloat, blue: CGFloat) -> UIColor {
        return UIColor(red: red/255, green: green/255, blue: blue/255, alpha: 1)
    }
    
}
```

<br>

# 10. 오토레이아웃 애니메이션
- 새로운 데이터가 Set 되는 순간, 특정 constraint를 deActive 하고, 다음차 우선순위 constraint가 적용되게끔 하는 테크닉

```
func setTextLabel(_ textData: String?, animated: Bool = true) {
  label.text = textData
  labelContainer.layoutIfNeeded()
  labelWidth.isActive = textData != nil
  UIView.animate(withDuration: animated ? 0.25 : 0, animations: layoutIfNeeded)
}
```

<br>

# 11. Data에 didSet을 통해 UI를 변경하기
- @RayWenderlich Devcon 2017. Lyft AutoLayout Guide

```
var separatorsVisible: Bool = false {
  didSet {
    horizontalSeparator.isHidden = !separatorsVisible
    verticalSeparator.isHidden = !separatorsVisible
  }
}
```