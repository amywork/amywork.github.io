---
layout: post
title: "Gesture Recognizer"
author: "Amy"
---

> [Gesture Recognizer](https://www.raywenderlich.com/162745/uigesturerecognizer-tutorial-getting-started)

# 스토리보드를 통한 연결
- Gesture Recognizer : View = 1:1 매칭
- 스토리보드를 통해 gestureRecognizer 오브젝트를 뷰와 연결 (잘 연결이 되어있는지 gestureRecognizers Outlet Collection 확인)
- 제스춰가 실행되었을 시의 callback function 정의 (샘플코드 출처: [www.raywenderlich.com](https://www.raywenderlich.com/162745/uigesturerecognizer-tutorial-getting-started))
- 스토리보드를 통해 제스춰 -> 해당 뷰콘트롤러로 콜백 함수 연결

{% highlight swift %}

@IBAction func handlePan(recognizer:UIPanGestureRecognizer) {
  let translation = recognizer.translation(in: self.view)
  if let view = recognizer.view {
    view.center = CGPoint(x:view.center.x + translation.x,
                            y:view.center.y + translation.y)
  }
  recognizer.setTranslation(CGPoint.zero, in: self.view)
}

{% endhighlight %}

# 샘플코드

{% highlight swift %}
import UIKit
class GestureVC: UIViewController, UIGestureRecognizerDelegate {

    @IBOutlet var topLabel:UILabel!
    @IBOutlet var secondLabel:UILabel!
    
    var count:String = ""
    var location:String = ""
    
    @IBAction func tapGesture(_ sender: UITouch) {
        //topLabel.text = count
        //secondLabel.text = location
    }
    
    func gestureRecognizer(_ gestureRecognizer: UIGestureRecognizer, shouldReceive touch: UITouch) -> Bool {
        count = "Tap Count : \(touch.tapCount) "
        location = "X 좌표: \(touch.location(in: self.view).x) " + "Y 좌표: \(touch.location(in: self.view).y)"
        topLabel.text = count
        secondLabel.text = location
        return true
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

}
{% endhighlight %}
