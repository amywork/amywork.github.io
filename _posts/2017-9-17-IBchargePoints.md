---
layout: post
title: "Swift로 UI 구현하기"
author: "younari"
---

### Swift로 간단한 UI 구현하기
- 일정 금액을 충전해서 물건을 구매하는 포인트몰을 구현.
- 금액 충전시 잔액 + 반영, 물건 구매시 잔액 - 반영.
- 코드 구성 과정은 // 코멘트로 표기.

![screenshot](younari.github.io/_images/0917chargeIB.png)

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
