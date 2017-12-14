---
layout: post
title: "RxSwift 실험 - RGB Slider"
author: "younari"
---

> RxSwift로 구현하는 작은 실험들 👀 a를 수정하면 b와 c의 값도 쉽게 변화해야 하는 상황일 때 Rx의 구원을 받아보자... ✨🙏🏻

## 목표
- Slider로 RGB 값을 0부터 1까지 조정하면서 이미지뷰의 Background Color와 Label의 RGB Value 값도 자동적으로 바뀌도록 구현한다.

## 1차 실험
- RGB 각각의 값을 관찰하기 위해 슬라이더도 3개 만들고, value도 3개 만들어서 각각 subscribe 시켜줬다. 뭔가 비슷한 코드를 계속 치고 있는 느낌이어서 코드가 훨씬 더 간단해질 수 있을 것 같다...☹️

{% highlight swift %}

import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {

    @IBOutlet weak var colorView: UIView!
    @IBOutlet weak var redSlider: UISlider!
    @IBOutlet weak var greenSlider: UISlider!
    @IBOutlet weak var blueSlider: UISlider!
    @IBOutlet weak var redValueLabel: UILabel!
    @IBOutlet weak var greenValueLabel: UILabel!
    @IBOutlet weak var blueValueLabel: UILabel!
    
    var disposeBag: DisposeBag = DisposeBag()
    
    var redValue: CGFloat = 0.5
    var greenValue: CGFloat = 0.5
    var blueValue: CGFloat = 0.5
    
    override func viewDidLoad() {
        super.viewDidLoad()
        colorView.backgroundColor = UIColor(red: redValue, green: greenValue, blue: blueValue, alpha: 1)
        bind()
    }

}

extension ViewController {
   
    func bind() {
        
        redSlider.rx.value.subscribe(onNext: { value in
            self.redValueLabel.text = "\(value)"
            self.redValue = CGFloat(value)
            self.setColor(r: CGFloat(value), g: self.greenValue, b: self.blueValue)
        })
        .disposed(by: disposeBag)
        
        greenSlider.rx.value.subscribe(onNext: { value in
            self.greenValueLabel.text = "\(value)"
            self.greenValue = CGFloat(value)
            self.setColor(r: self.redValue, g: CGFloat(value), b: self.blueValue)
        }).disposed(by: disposeBag)
        
        blueSlider.rx.value.subscribe(onNext: { value in
            self.blueValueLabel.text = "\(value)"
            self.blueValue = CGFloat(value)
            self.setColor(r: self.redValue, g: self.greenValue, b: CGFloat(value))
        }).disposed(by: disposeBag)
        
    }
    
    func setColor(r: CGFloat, g: CGFloat, b: CGFloat) {
        colorView.backgroundColor = UIColor(red: r, green: g, blue: b, alpha: 1)
    }
    
}

{% endhighlight %}
