---
layout: post
title: "스위프트 Scroll View"
author: "younari"
---

# UI Scroll View
- [Xcode 스위프트 프로젝트 바로가기](https://github.com/younari/tastySwift)
- UIImageView, UILabel, UIControl, UIWindow, **UIScrollView**
- UIButton, UISlider, UISwitch, UITextField
- ScrollView 내에 존재하는 프로퍼티인 ContentView 가 움직이는 것.
- **ScrollView 및 ContentView의 사이즈를 모두 설정해야 한다.**

1. UIScrollView의 인스턴스 생성 `scrollView = UIScrollView(frame: rectStyle)`
2. `class ViewController: UIViewController, UIScrollViewDelegate` 델리게이트 지정
3. `scrollView.delegate = self` 델리게이트를 self로 선언
4. `scrollView.contentSize = CGSize(` 콘텐트사이즈 설정
5. `view.addSubview(scrollView)` view에 add

{% highlight swift %}
scrollView = UIScrollView(frame: rectStyle)
scrollView.delegate = self // UIScrollViewDelegate
scrollView.contentSize = CGSize(width: scrollView.frame.size.width*3, height: scrollView.frame.size.height)
view.addSubview(scrollView)
{% endhighlight %}


# contentOffset을 사용한 Color Transition 
- `scrollView.contentOffset.x`

{% highlight swift %}
func scrollViewDidScroll(_ scrollView: UIScrollView) {
    let offsetX: CGFloat = scrollView.contentOffset.x
     let colorValue: CGFloat = 1 - (offsetX/scrollView.contentSize.width)
    for i in 0..<3 {
        subViewArr[i].backgroundColor = UIColor(red: 1, green: colorValue, blue: colorValue, alpha: 1)
    }
}
{% endhighlight %}



# setContentOffset  
- 콘텐츠뷰와 스크롤뷰의 교차 좌표, 어디서부터 스크롤을 시작할 것인지 결정
- `scrollView.setContentOffset()`
{% highlight swift %}
override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)
    scrollView.setContentOffset(CGPoint(x: 0, y: 0), animated: true)
}
{% endhighlight %}


# 샘플 코드 v0.1

{% highlight swift %}
import UIKit
class ViewController: UIViewController {
    
    var scrollView: UIScrollView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        let rectStyle = CGRect(x: 0, y: 0, width: view.frame.size.width, height: 500)
        scrollView = UIScrollView(frame: rectStyle)
        scrollView.contentSize = CGSize(width: 1000, height: scrollView.frame.size.height)
        scrollView.bounces = true // defalut : true
        scrollView.isPagingEnabled = false // default : false, page = scrollView size
        view.addSubview(scrollView)
        
        for n in 0..<50 {
            let row = CGFloat(n/10)
            let column = CGFloat(n%10)
            let subView: UIView = UIView(frame: CGRect(x: column*100, y: row*100, width: 100, height: 100))
            subView.backgroundColor = UIColor(red: column*13/255.0, green: row*25/255.0, blue: 1.0, alpha: 1)
            scrollView.addSubview(subView)
        }
    }
    
{% endhighlight %}

# 샘플 코드 v0.2

{% highlight swift %}
import UIKit

class ViewController: UIViewController, UIScrollViewDelegate {
    
    var scrollView: UIScrollView!
    var subView: UIView = UIView()
    var subViewArr: [UIView] = []
    
    override func viewDidLoad() {
        // viewDidLoad => 메모리에 view가 로드되는 시점
        super.viewDidLoad()
        let rectStyle = CGRect(x: 0, y: 0, width: view.frame.size.width, height: view.frame.size.height)
        scrollView = UIScrollView(frame: rectStyle)
        scrollView.delegate = self // UIScrollViewDelegate
        scrollView.contentSize = CGSize(width: scrollView.frame.size.width*3, height: scrollView.frame.size.height)
        view.addSubview(scrollView)
        
        for n in 0..<3 {
            subView = UIView()
            subView.tag = n
            subView.frame = CGRect(x: CGFloat(n)*view.frame.size.width, y: 0, width: view.frame.size.width, height: view.frame.size.height)
            scrollView.addSubview(subView)
            subView.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
            subViewArr.append(subView)
        }
        

    }
    
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        let offsetX: CGFloat = scrollView.contentOffset.x
        let colorValue: CGFloat = 1 - (offsetX/scrollView.contentSize.width)
        for i in 0..<3 {
            subViewArr[i].backgroundColor = UIColor(red: 1, green: colorValue, blue: colorValue, alpha: 1)
        }
    }
    
    // viewDidAppear 이후에 setContentOffset 애니메이션 추가
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        scrollView.setContentOffset(CGPoint(x: 0, y: 0), animated: true)
        // Offset: 콘텐츠뷰와 스크롤뷰의 교차 좌표, 얼마나 더 가서 시작할 것인지 결정
    }
}
{% endhighlight %}


# Color Card Transition
- 컬러를 배열에 담아, 여러가지 컬러의 카드를 스크롤하는 인터렉션

{% highlight swift %}
import UIKit
class CardScrollViewController: UIViewController, UIScrollViewDelegate {
    @IBOutlet weak var todayLabel: UILabel!
    @IBOutlet weak var cardScrollView: UIScrollView!

    override func viewDidLoad() {
        super.viewDidLoad()
        cardScrollView.contentSize = CGSize(width: cardScrollView.frame.size.width*10, height: cardScrollView.frame.size.height)

        var colors: [UIColor] = [.red, .blue, .green] // 컬러담기      
        for n in 0..<colors.count {
            let card = UIView()
            var offset: CGFloat {
                return 16 + (260*CGFloat(n)) + (16*2*CGFloat(n))
            }
            card.frame = CGRect(x: offset, y: 16, width: 260, height: cardScrollView.frame.size.height-32)
            card.backgroundColor = colors[n]
            card.layer.cornerRadius = 13
            cardScrollView.addSubview(card)
        }
        
    }
}
{% endhighlight %}



# UISwitch
- `UISwitch` 인스턴스 생성
- `addTarget` 설정
- 예시: `action: #selector(ViewController.colorChange)`
- `for: .valueChange` 컨트롤 스테이트 (isOn)
- `addSubview`


{% highlight swift %}
let colorSwitcher: UISwitch = UISwitch(frame: CGRect(x: 10, y: view.frame.size.height-36, width: 20, height: 20))
colorSwitcher.addTarget(self, action: #selector(ViewController.colorChange), for: .valueChanged)
self.view.addSubview(colorSwitcher)
{% endhighlight %}

{% highlight swift %}
@objc func colorChange(_ sender: UISwitch) {
    if sender.isOn {
        view.backgroundColor = #colorLiteral(red: 0.9607843161, green: 0.7058823705, blue: 0.200000003, alpha: 1)
    }else {
        view.backgroundColor = #colorLiteral(red: 1, green: 0.331463933, blue: 0.3345415592, alpha: 1)
    }
}
{% endhighlight %}


