---
layout: post
title: "Delegate Pattern"
author: "younari"
---

# Delegate Pattern
### Sources from
- [Swift4 - Protocols](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html)

# Protocol
- 프로토콜은 method, property 등으로 요구 사항의 청사진을 정의한다.
- 클래스, 구조체, 열거형이 프로토콜을 채택했을 때, 프로토콜에서 요구한 사항에 대해 구현해야 한다.- 프로토콜을 통해 공통적인 **작업을 강제** 할수 있다.
- 실제 구현은 Class, Struct, Enum에서 한다.
- 프로토콜을 타입처럼 사용 가능하다.
- If a protocol requires a property to be gettable and settable, that property requirement can’t be fulfilled by a constant stored property or a read-only computed property. If the protocol only requires a property to be gettable, the requirement can be satisfied by any kind of property, and it’s valid for the property to be also settable if this is useful for your own code.

## Example

{% highlight swift %}
protocol Runable {	var regCount:Int {get set} // regCount 프로퍼티를 무조건 만들어야 하며, getter, setter를 모두 구현해야 한다. 	func run()
}
{% endhighlight %}


# Delegate
- 클래스나 구조체에서 할 일의 일부를 다른 인스턴스에게 대신 하게 하는 디자인 패턴

<br>

# Sample code 01. UITableView

## UITableViewDelegate 프로토콜 선언

{% highlight swift %}
public protocol UITableViewDelegate : NSObjectProtocol, UIScrollViewDelegate {

    // Display customization
    
    @available(iOS 2.0, *)
    optional public func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath)

    @available(iOS 6.0, *)
    optional public func tableView(_ tableView: UITableView, willDisplayHeaderView view: UIView, forSection section: Int)

    @available(iOS 6.0, *)
    optional public func tableView(_ tableView: UITableView, willDisplayFooterView view: UIView, forSection section: Int)

    @available(iOS 6.0, *)
    optional public func tableView(_ tableView: UITableView, didEndDisplaying cell: UITableViewCell, forRowAt indexPath: IndexPath)

...

{% endhighlight %}

## UITableView 클래스에 delegate 프로퍼티 생성
- UITableView는 View 스스로가 결정할 수 없는 부분을 ViewController에게 위임하고자 한다. (ex. cellForRowAt)
- 따라서 UITableViewDelegate 프로토콜을 채택한 인스턴스를 저장할 delegate 프로퍼티를 갖고 있다.

{% highlight swift %}
open class UITableView : UIScrollView, NSCoding, UIDataSourceTranslating {

    
    public init(frame: CGRect, style: UITableViewStyle) // must specify style at creation. -initWithFrame: calls this with UITableViewStylePlain

    public init?(coder aDecoder: NSCoder)

    
    open var style: UITableViewStyle { get }

    
    weak open var dataSource: UITableViewDataSource?

    weak open var delegate: UITableViewDelegate?
    
    
    ...
    
{% endhighlight %}


## 구현부
- ViewController에서 UITableViewDelegate 프로토콜을 채택 → 
- ViewController는 UITableViewDelegate 프로토콜이 강제하는 메소드를 모두 정의 → 
- UITableView 의 인스턴스인 tableView 생성 → 
- tableView의 delegate에 자신(ViewController)을 대입

{% highlight swift %}

class ViewController: UIViewController, UITableViewDataSource, UITableViewDelegate, UIScrollViewDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
        let tableView: UITableView = UITableView(frame: view.bounds, style: .plain)
        tableView.dataSource = self // UITableViewDataSource 선언
        tableView.delegate = self // UITableViewDelegate 선언
        
        // class 로서 tableView에 등록
        tableView.register(Cell.self, forCellReuseIdentifier: "Cell")
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "UITableViewCell")
        // UITableViewCell.self : class이름.self = 클래스 자체
        // dequeueReusableCell 얘가 UI를 만들 수 있도록 등록하는 과정
        view.addSubview(tableView)
    }
    
{% endhighlight %}



# Sample code 02. Custom View

## CustomViewDelegate 프로토콜 생성
- 스위치의 value change에 따라 뷰의 프로퍼티를 변환시킬 메소드 정의

{% highlight swift %}
protocol CustomViewDelegate {
    func didSwitchedCustomView(_ customView: CustomView)
    func didDeSwitchedCustomView(_ customView: CustomView)
}
{% endhighlight %}


## UIView인 CustomView 클래스에 delegate 정의
- var delegate: CustomViewDelegate?
- switch가 isOn이면 delegate?.didSwitchedCustomView(self) 실행
- switch가 isOff이면 delegate?.didDeSwitchedCustomView(self) 실행

{% highlight swift %}

import UIKit
class CustomView: UIView {
    
    var delegate: CustomViewDelegate?
    
    @IBOutlet var titleLabel: UILabel!
    @IBOutlet var imageView: UIImageView!
    @IBOutlet var switchBtn: UISwitch!
    
    @IBAction func switching(_ sender: UISwitch) {
        if sender.isOn {
            delegate?.didSwitchedCustomView(self)
        }else {
            delegate?.didDeSwitchedCustomView(self)
        }
    }
    
    override func awakeFromNib() {
        super.awakeFromNib()
        switchBtn.isOn = false
        imageView.contentMode = UIViewContentMode.center
    }
    
}
{% endhighlight %}

## ViewController에서 실제 메소드 구현
- ViewController에서 CustomViewDelegate 프로토콜 채택
- CustomView의 인스턴스인 newCustomView 생성
- newCustomView의 delegate에 self(ViewController) 지정
- CustomViewDelegate 프로토콜이 강제하는 메소드 정의


{% highlight swift %}

import UIKit

class ViewController: UIViewController, CustomViewDelegate {
    
    @IBOutlet var newCustomView: CustomView!
    var starimage: UIImage?
    
    func didSwitchedCustomView(_ customView: CustomView) {
        customView.titleLabel.text = "샌프란시스코로 떠나시겠습니까?"
        customView.imageView.image = nil
    }
    
    func didDeSwitchedCustomView(_ customView: CustomView) {
        customView.titleLabel.text = "Welcome to AirBnb"
        customView.imageView.image = UIImage(named: "Logo")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        newCustomView.delegate = self
        starimage = UIImage(named: "staricon")
    }
    
}
{% endhighlight %}
