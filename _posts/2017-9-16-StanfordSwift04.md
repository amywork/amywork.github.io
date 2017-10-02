---
layout: post
title: "스탠포드 iOS 강의노트 L6,7"
author: "younari"
---

## Lecture 6,7
### Multiple MVCs, LifeCycle, ARC, Protocol, Delegation

> **Course Description** Updated for iOS 10 and Swift. Tools and APIs required to build applications for the iPhone and iPad platforms using the iOS SDK. User interface design for mobile devices and unique user interactions using multi-touch technologies. Object-oriented design using model-view-controller paradigm, memory management, Swift programming language. Other topics include: object-oriented database API, animation, mobile device power management, multi-threading, networking and performance considerations.

- This course material is only available in the iTunes U app on iPhone or iPad.
- [Developing iOS 10 Apps with Swift
by Stanford](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120)


# Multiple MVCs
- 2개 이상의 ViewController를 만들고, segue를 통해 화면을 전환하여 테스크를 분리합니다.
- Lecture 5에 이어서,감정을 선택할 수 있는 게이트웨이 역할을 하는 메인 EmotionViewController를 추가합니다.

{% highlight swift %}
import UIKit
class EmotionsViewController: VCLLoggingViewController
{
	// override func prepare(for segue: )
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        var destinationViewController = segue.destination
        
        if let navigationController = destinationViewController as? UINavigationController {
            destinationViewController = navigationController.visibleViewController ?? destinationViewController
        }
        
        // 화면 전환에 따른 expression 변화
        if let faceViewController = destinationViewController as? FaceViewController,
            let identifier = segue.identifier,
            let expression = emotionalFaces[identifier] {
                 faceViewController.expression = expression
            faceViewController.navigationItem.title = (sender as? UIButton)?.currentTitle
                }
            }
 	
 	// segue identifier와 FacialExpression(Model)의 case 매칭하여 Dic 형태로 저장
   
    private let emotionalFaces: Dictionary<String,FacialExpression> = [
        "happy": FacialExpression(eyes: .open, mouth: .smile),
        "sad": FacialExpression(eyes: .open, mouth: .frown),
        "worried": FacialExpression(eyes: .closed, mouth: .smirk)
    ]
}
{% endhighlight %}


# View Controller LifeCycle (요약)
- Instantiated (from storyboard usually)- awakeFromNib()- segue preparation happens outlets get set `viewDidLoad`- These pairs will be called each time your Controller’s view goes on/off screen ... 
- `viewWillAppear` and `viewDidAppear`- `viewWillDisappear` and `viewDidDisappear`- These “geometry changed” methods might be called at any time after viewDidLoad ... 
- `viewWillLayoutSubviews` (... then autolayout happens, then ...) `viewDidLayoutSubviews`- If memory gets low, you might get ... `didReceiveMemoryWarning`

# Automatic Reference Counting- Reference types (classes) are stored in the heap.- How does the system know when to reclaim the memory for these from the heap?- It “counts references” to each of them and when there are zero references, they get tossed. This is done automatically.- It is known as **“Automatic Reference Counting”** and it is NOT garbage collection.# Influencing ARC- You can influence ARC by how you declare a reference-type var with these keywords ...- **strong**
- **weak**
- **unowned**

# Protocols
- A way to express an API more concisely.
- Collection of Methods and Property Declarations.
- A Protocol is a TYPE.
- The implementation is provided by an implementing type(any class, struct, or enum)
- have no storage.  

# Delegation
- A very Simple & Important use of Protocols.
- It is a way to implement "blind communication" between a View and a Controller.
- View ->(delegate, datasource)-> Controller
- delegate `should` `did` `will`
- datasource `data at` `count`

### How it plays out
1. A View declares a delegation protocol (what the View wants the controller to do for it)
2. The View's API has a weak delegate property whose type is that delegation protocol
3. The View uses the delegate property to get/do things it can't own or control on its own
4. The Controller declares that it implements the protocol
5. The Controller sets self as the delegate of the View by setting the property in @2
6. The Controller implements the protocol (probably it has lots of optional methods in it)

# Scroll View
- `func scrollRectToVisibe(CGRect, animeted: Bool)`
- `scrollView.minimumZoomScale = 0.5`
- `scorllView.maximumZoomScale = 2.0`
- `func viewForZooimg(in scrollView: UIScrollView) -> UIView`