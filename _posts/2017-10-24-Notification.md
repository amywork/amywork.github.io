---
layout: post
title: "Notification Center"
author: "Amy"
---
> [Notification Center Offical Documents](https://developer.apple.com/documentation/foundation/notificationcenter), [Notification Programming Topics](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html#//apple_ref/doc/uid/10000043i)

- 인스턴스 간의 데이터를 주고받는 방식
- **발송자(Notification Center)**: 특정 이벤트가 발생 하였음을 불특정 다수의 객체에게 알리기 위해 사용하는 클래스
- **수신자(Observer)**: 어떤 객체라도 특정 이벤트가 발생했다는 알림을 받을 것이라고 관찰자(Observer)로 등록을 해두면 노티피케이션 센터가 모든 관찰자 객체에게 알림을 준다.
- UserDefaults처럼 모든 app이 하나씩 가지고 있다. 
- **Each running app has a default notification center**, and you can create new notification centers to organize communications in particular contexts.

### class var `default`: NotificationCenter { get }

- The app’s default notification center.
- All system notifications sent to an app are posted to the default notification center. You can also post your own notifications there.
If your app uses notifications extensively, you may want to create and post to your own notification centers rather than posting only to the default notification center. When a notification is posted to a notification center, the notification center scans through the list of registered observers, which may slow down your app. By organizing notifications functionally around one or more notification centers, less work is done each time a notification is posted, which can improve performance throughout your app.

### Adding and Removing Notification Observers
- func `addObserver`(forName: NSNotification.Name?, object: Any?, queue: OperationQueue?, using: (Notification) -> Void)
- func `addObserver`(Any, selector: Selector, name: NSNotification.Name?, object: Any?)
- func `removeObserver`(Any, name: NSNotification.Name?, object: Any?)


### Posting Notifications
- func `post`(Notification)
- func `post`(name: NSNotification.Name, object: Any?)
Creates a notification with a given name and sender and posts it to the notification center.


# Sample Code
### 포스팅
{% highlight swift %}
@IBAction func notiPostBtn(_ sender: UIButton) {
        NotificationCenter.default.post(name: NSNotification.Name.init("TestNoti"), object:ThirdTF!.text, userInfo: ["noti":"info"])
    }
{% endhighlight %}

### 옵저빙
{% highlight swift %}
override func viewDidLoad() {
    super.viewDidLoad()
    NotificationCenter.default.addObserver(forName: Notification.Name.init("TestNoti"), object: nil, queue: nil) { (noti) in
        if let text = noti.object as? String {
            self.secondLB.text = text
        }
    }
}
{% endhighlight %}
