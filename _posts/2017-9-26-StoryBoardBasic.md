---
layout: post
title: "스토리보드로 UI 구현하기"
author: "Amy"
---

# StoryBoard
> A file that contains a visual representation of the app’s UI (user interface), showing screens of content and the transitions between them, that you work on in Interface Builder.

## storyboard entry point
- The first scene that’s shown from a storyboard when an app starts.

## UIView
- The elements that appear in the user interface are known as views. Views display content to the user. They are the building blocks for constructing your user interface and presenting your content in a clear, elegant, and useful way. Views have a variety of useful built-in behaviors, including displaying themselves onscreen and reacting to user input.
- All view objects in iOS are of type UIView or one of its subclasses. Many UIView subclasses are highly specialized in appearance and behavior. 

###### Sources from [Developer.apple.com](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/GlossaryDefinitions.html#//apple_ref/doc/uid/TP40015214-CH12-SW10)



## 실습: Xcode 스토리보드로 간단한 UI 구현하기
- 일정 금액을 충전해서 물건을 구매하는 포인트몰을 구현.
- 금액 충전시 잔액 + 반영, 물건 구매시 잔액 - 반영.
- [스크린샷 보러가기](https://github.com/amywork/tastySwift/blob/master/0916_appleMachine/Readme.md)

{% highlight swift %}

class ViewController: UIViewController {

    // viewDidLoad시 초기화
    override func viewDidLoad() {
        super.viewDidLoad()
        displayLabel.text = "0원"
        notification.text = nil
    }
 
    // 잔액을 보여주는 UILable
    @IBOutlet weak var displayLabel: UILabel!

    // 구매 성공 또는 실패를 나타내는 노티피케이션 UILabel
    @IBOutlet var notification: UILabel!
    
  
    // 충전 또는 구매에 의해 변하는 잔액 변수
    var chargeValue: Int = 0
    

    
    // 금액 충전 버튼(btnCharge) : UIbutton
    // 3종류: 10만원, 30만원, 50만원 *Int 태그 사용
    // 기능: btnCharge 클릭 시 chargeValue에 금액 추가, displayLabel 값 변경
    @IBAction func chargePoints(btnCharge: UIButton) {
        chargeValue += Int(btnCharge.tag)
        displayLabel.text = String(chargeValue) + "원"
        displayLabel.tag = chargeValue
    }
    
    
    // 물건 구매 버튼(btnBuy) : UIButton
    // 4가지 종류: 5만원, 10만원, 20만원, 30만원 *Int 태그 사용
    // 01. 잔액 >= 물건 금액 -> 구매 완료 노티피케이션, 잔액 -= 물건금액
    // 02. 잔액 < 물건 금액 -> 구매 실패, 잔액 충전 노티피케이션
    @IBAction func buyItem(btnBuy: UIButton) {
        if chargeValue >= Int(btnBuy.tag) {
            notification.text = "구매가 성공적으로 완료되었습니다!"
            chargeValue -= btnBuy.tag
            displayLabel.text = String(chargeValue) + "원"
        }else {
            notification.text = "잔액 부족😂 포인트를 충전해주세요!"
        }
    }
}

{% endhighlight %}
