---
layout: post
title: "RxSwift 미니 실험"
author: "younari"
---

> **Rx Swift**로 구현하는 작은 실험들 👀 하나의 값이 변할 때 다른 값들에도 미치는 영향도가 높을 때 Rx의 구원을 받아보자 😇

# RGB Slider 만들기
- Slider로 RGB 값을 0부터 1까지 조정하면서 이미지뷰의 Background Color와 Label의 RGB Value 값도 자동적으로 바뀌도록 구현한다.

## ✔️ 1차 실험
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

## ✔️ 2차 실험
- 강사님의 코드를 보고 수정 -> Reactive where Base: UIView 를 extension 해서, backgroundColor를 Binder로 만들어서, Observable<UIColor> 를 colorView.rx.backgroundColor 에 bind 👏🏻

{% highlight swift %}

// extension Reactive
extension Reactive where Base: UIView {
    var backgroundColor: Binder<UIColor> {
        return Binder(self.base) { view, color in
            view.backgroundColor = color
        }
    }
}

let color = Observable<UIColor>.combineLatest(redSlider.rx.value, greenSlider.rx.value, blueSlider.rx.value) { (r,g,b) -> UIColor in
            return UIColor(displayP3Red: CGFloat(r), green: CGFloat(g), blue: CGFloat(b), alpha: 1)
        }
        
        color.bind(to: colorView.rx.backgroundColor).disposed(by: disposeBag)

{% endhighlight %}

# Rx 구구단 만들기
- textField에 숫자(n)를 입력하면, 해당 구구단(1xn ~ 9xn)을 label에 바로 출력하게 한다.

#### ✔️ 나의 풀이 :: textField를 rx 로 바꿔서 For 문을 돌려 구구단을 만들고 label에 bind 시켰다.

{% highlight swift %}

func bind() {
    textField.rx.text.orEmpty.asObservable()
        .map({ (str) -> String in
            guard let number = Int(str) else { return "" }
            var result: String = ""
            for i in 1...9 {
                result += "✏️ \(i) × \(number) = \(i*number) \n"
            }
            return result
        })
        .bind(to: label.rx.text).disposed(by: disposeBag)
}

{% endhighlight %}

#### ✔️ 다른 사람의 풀이 :: 배열을 만들어 map -> reduce -> bind

{% highlight swift %}

func bind2() {
    textField.rx.text.orEmpty.asObservable()
        .map({ (str) -> String in
            guard let number = Int(str) else { return "" }
            var result: String = ""
            let numArr = [1,2,3,4,5,6,7,8,9]
            result = numArr.map({ (num) -> String in
                return "\(number) * \(num) = \(number*num)"
            }).reduce("", {$0 + "\n" + $1})
            return result
        })
        .bind(to: label.rx.text).disposed(by: disposeBag)
}
    
{% endhighlight %}


#### ✔️ 강사님의 풀이 :: Observable을 리턴하여 바인드

{% highlight swift %}

func bind3() {
    textField.rx.text.orEmpty.asObservable()
        .flatMap({(value) -> Observable<Int> in
            guard let result = Int(value) else { return Observable.empty() }
            return Observable.just(result)
        }).flatMap({(dan: Int) -> Observable<[String]> in
            Observable.from([1,2,3,4,5,6,7,8,9]).map({ (num) -> String in
                return "\(dan) * \(num) = \(dan*num)"
            }).toArray()
        }).map({ steps -> String in
            return steps.reduce("", { (prev: String, next: String) -> String in
                return prev + "\n" + next
            })
        })
        .bind(to: label.rx.text).disposed(by: disposeBag)
}
    
{% endhighlight %}
