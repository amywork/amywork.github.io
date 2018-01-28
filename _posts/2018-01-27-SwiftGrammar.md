---
layout: post
title: "Swift note"
author: "younari"
---

> 정확하게 몰랐던 내용들, 지나가다 주워 들은 내용들 미니 노트에 필기하기 🙂🙃

# ScrollView
- `scrollRectToVisible(_:animated:)`
- [Apple Doc](https://developer.apple.com/documentation/uikit/uiscrollview/1619439-scrollrecttovisible#declarations)
- [stackoverflow](https://stackoverflow.com/questions/1446536/uiscrollview-works-as-expected-but-scrollrecttovisible-does-nothing)

# Xcode config
- [Using xcconfig files](http://www.jontolof.com/cocoa/using-xcconfig-files-for-you-xcode-project/)

# init rect & coder
- init(rect:)는 뷰를 코드로 생성할때 사용하는 방식
- init(coder:)는 뷰를  xib로 생성할때 사용하는 방식
- 예시로 아래에서는 스토리보드를 통해 init 했으므로, init(coder:)가 불림

```
collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "MyIdentifier", for: indexPath) 
```

# AutoLayout
- setNeedsLayout / layoutIfNeeded / layoutSubviews의 차이점은 뭘까?
- setNeedsLayout: asynchronous update (비동기 업데이트)
- layoutIfNeeded: synchronous update (동기 업데이트 = 바로 실행)
- layoutSubviews: override method
- (발췌) So, stated succinctly, layoutIfNeeded says update immediately please, whereas setNeedsLayout says please update but you can wait until the next update cycle.
- [참고자료01](http://www.iosinsight.com/setneedslayout-vs-layoutifneeded-explained/)
- [참고자료02](https://medium.com/@abhimuralidharan/ios-swift-setneedslayout-vs-layoutifneeded-vs-layoutsubviews-5a2b486da31c)


# UIColor Extension
- UIColor RGB로 컬러 만들기

```
extension UIColor {
    
    static func rgb(_ red: CGFloat, green: CGFloat, blue: CGFloat) -> UIColor {
        return UIColor(red: red/255, green: green/255, blue: blue/255, alpha: 1)
    }
    
}
```

