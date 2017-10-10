---
layout: post
title: "iOS View Controller"
author: "younari"
---

# UIViewController

![segue](https://younari.github.io/images/Segue.jpg)

- 모든 앱은 적어도 한 개 이상의 UIViewController를 가지고 있어야 하며, 대부분의 앱은 여러개의 UIViewController로 이뤄져 있다.
- **모든 앱은 하나의 Root View Controller를 가지고 있고, 모든 View Controller는 제각각 자신의 Root View를 가지고 있다.**
- UIViewController는 사용자의 **인터렉션과 앱의 데이터 사이에서** 컨트롤의 역할을 한다.
- UIViewController는 **모든 View의 관리, 사용자 이벤트 핸들링, UIViewController간의 전환** 등의 역할을 수행한다.
- 모든 UIViewController는 한 개의 **RootView**를 필수적으로 가지고 있으며, 화면에 표시되는 모든 View는 RootView의 SubView로 존재한다.
- Window 위에 1개의 Tapbar 올릴 수 있으며, Tapbar 는 여러개의 ViewController를 Sub로 가질 수 있고, 그마다 각각의 NavigationViewController를 가질 수 있다.

### 👌🏻UIViewController의 Instance 만들기

- Class를 직접 인스턴스 시키는 방법, 단 스토리보드와 연관되지 않는다.

{% highlight swift %}
let secondVC1 = SecondViewController()
{% endhighlight %}

- 따라서 스토리보드를 통해 UI를 만들어놓은 ViewController의 인스턴스를 생성하여 present해야 한다.

{% highlight swift %}

// 스토리보드 파일을 통하여,
let storyboard = UIStoryboard(name: “Storyboard Name”, bundle: nil) 

// ViewController를 인스턴스화 해서, present 한다.
let vc:UIViewController = storyboard.instantiateViewController(withIdentifier: "Storyboard Identifier")

present(vc, animated: true, completion: nil)

{% endhighlight %}


# General ViewController
- UIViewController(Root = UIView), UITableViewController(Root = UITableView), UICollectionViewController(Root = UICollectionView)


# Navigation ViewController
- UINavigationController(Stack), UITabbarController(Switching), UISplitViewController(Splitting)


# Present Modally
- 기준 ViewController에서 대상 ViewController를 Present하고, dismiss를 통해 대상 ViewController는 사라진다.

### present
`present(UIViewController, animated: Bool, completion: (() -> Void)?)`

### dismiss
`dismiss(animated: Bool, completion: (() -> Void)?`

{% highlight swift %}
func close(_ sender: UIButton) {
    dismiss(animated: true, completion: nil)
}    
{% endhighlight %}

![segue](https://raw.githubusercontent.com/younari/younari.github.io/master/images/SegueProcess.png)

# Segue
- Storyboard 파일 내 두 개의 ViewController사이 화면전환을 정의한 인스턴스
- 시작점은 UIButton, UITableView의 Selected Row, UIGesture등
- 종착점은 UIViewController
- Segue 또한 Identifier를 가질 수 있다.

{% highlight swift %}
import UIKit
class SegueTestViewController: UIViewController {

    var isAbleToNext: Bool = true
    
    // UI스위치 액션에 대한 Bool 값 (On/Off)을 가지고 segue의 perform 여부를 결정한다.
    @IBAction func ableToNext(_ sender: UISwitch) {
        isAbleToNext = sender.isOn
    }
    
    // "shouldPerformSegue"
    override func shouldPerformSegue(withIdentifier identifier: String, sender: Any?) -> Bool {
        if identifier == "nextDestination" {
            return isAbleToNext
        }else {
            return false
        }
    }
}
{% endhighlight %}
