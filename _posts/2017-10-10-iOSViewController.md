---
layout: post
title: "iOS View Controller"
author: "Amy"
---

# UIViewController

> [샘플 Xcode 프로젝트 바로가기](https://github.com/amywork/tastySwift/tree/master/1011_ViewController)

![segue](https://amywork.github.io/images/Segue.jpg)

- 모든 앱은 적어도 한 개 이상의 UIViewController를 가지고 있어야 하며, 대부분의 앱은 여러개의 UIViewController로 이뤄져 있다.
- **모든 앱은 하나의 Root View Controller를 가지고 있고, 모든 View Controller는 제각각 자신의 Root View를 가지고 있다.**
- UIViewController는 사용자의 **인터렉션과 앱의 데이터 사이에서** 컨트롤의 역할을 한다.
- UIViewController는 **모든 View의 관리, 사용자 이벤트 핸들링, UIViewController간의 전환** 등의 역할을 수행한다.
- Window 위에 1개의 Tapbar 올릴 수 있으며, Tapbar 는 여러개의 ViewController를 Sub로 가질 수 있고, 그마다 각각의 NavigationViewController를 가질 수 있다.

## UIViewController의 Instance 만들기

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


## 👍🏻 NSCoder
- NSCoder를 이용해서 XML 파일로 만든 스토리보드를 바이트 단위의 Data로 아카이빙 시켜준 뒤에, 변환된 Data를 다시 `init(coder aDecoder: NSCoder)`로 디코딩하여 뷰콘트롤러의 인스턴스를 만든다.
- instance가 만들어지기 이전에 호출

{% highlight swift %}
init(coder aDecoder: NSCoder) {
        <#code#>
}
{% endhighlight %}


## 👍🏻 awakeFromNib
- instance가 만들어진 후에 호출 (IBOutlet, IBAction 스토리보드의 View가 모두 바인딩 됨)
- awakeFromNib가 호출되는 시점에는 IBAction과 IBOutlet이 연결되어 있으므로 이들은 nil이 아님을 보장한다.

- nib을 사용하는 경우 불려지는 순서
- init -> initWithCoder -> awakeFromNib

- nib를 사용하지않는 경우 불려지는 순서
- init -> initWithFrame
- nib 파일 내 오브젝트들은 initWithFrame이 호출되지 않는다.


# General ViewController
- UIViewController(Root = UIView), UITableViewController(Root = UITableView), UICollectionViewController(Root = UICollectionView)


# Navigation ViewController
- UINavigationController(Stack), UITabbarController(Switching), UISplitViewController(Splitting)


# Present Modally
- 기준 ViewController에서 대상 ViewController를 Present하고, dismiss를 통해 대상 ViewController는 사라진다.
- `presentedViewController`, `presentingViewController`


### present
`present(UIViewController, animated: Bool, completion: (() -> Void)?)`

### dismiss
`dismiss(animated: Bool, completion: (() -> Void)?`

{% highlight swift %}
func close(_ sender: UIButton) {
    dismiss(animated: true, completion: nil)
}    
{% endhighlight %}


# Segue
- Storyboard 파일 내 두 개의 ViewController 사이 화면 전환을 정의한 인스턴스를 Segue라고 한다.
- 시작점은 UIButton, UITableView의 Selected Row, UIGesture등이 된다.
- 종착점은 UIViewController가 된다.
- Segue 또한 Identifier를 가질 수 있다.

![segue](https://raw.githubusercontent.com/amywork/amywork.github.io/master/images/SegueProcess.png)


### should Perform Segue?
- Segue를 통한 화면전환을 해야할지에 대한 정의

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

### prepare for Segue?
- prev -> next 뷰콘트롤러로 보낼 데이터 등을 정의

{% highlight swift %}
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if segue.identifier == "nextDestination" {
        let destionationVC = segue.destination as? DestinationViewController
        let nextStr = textField.text ?? "nothing to send"
        destionationVC?.takeMessage(nextStr) // String 전달
    }
}
{% endhighlight %}


### Manual Segue (수동 segue)
- UIButton같은 trigger가 없이 화면을 이동해야 할 떄
- `performSegue(withIdentifier: "id", sender: self)`


### Unwind Segue
- Exit을 통한 unwind를 했을 때, **돌아가야 할 ViewController에** 아래의 @IBAction을 만들어준다.
- **Exit을 하는 ViewController에서** 버튼을 통한 Exit segue를 만들어준다.

{% highlight swift %}
@IBAction func dismissCompletion(_ sender: UIStoryboardSegue) {
	// tableView를 reload 한다던가 등등의 작업을 해줍니다.
	// 아무 작업을 해주지 않으면 원래 그상태로의 화면이 남아있습니다.
}
{% endhighlight %}