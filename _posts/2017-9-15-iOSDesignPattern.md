---
layout: post
title: "iOS Design Pattern"
author: "younari"
---

### iOS Design Pattern 
- 현재 작성중인 파일로 이 문구가 살아있는 한 계속 수정될 수 있습니다.
- 사용자 클릭 -> OS -> UI Application -> .. -> View -> View Controller -> model..
- 원래 순서는 delegate -> viecController 이동인데 스토리보드라는 변수가 생김

- StoryBoard는 XML 파일로 작성되어 있다.
- 이벤트 Loop 은 Que 구조(선입선출)로 이벤트들이 쌓여 하나씩 빠르게 처리 된다.


### AppDelegate
- UIApplicationMain이 AppDelegate를 호출
- didFinishLaunchingWithOptions : 어플리케이션 런칭 후 커스터마이징

{% highlight swift %}

@UIApplicationMain

class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.
    return true
    }

    //
    func applicationWillResignActive(_ application: UIApplication) {
    // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
    // Use this method to pause ongoing tasks, disable timers, and invalidate graphics rendering callbacks. Games should use this method to pause the game.
    }

    // 여기에 주로 terminate 될 때의 데이터 저장 등의 기능들을 정의함
    func applicationDidEnterBackground(_ application: UIApplication) {
    // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
    // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
    }

    func applicationWillEnterForeground(_ application: UIApplication) {
    // Called as part of the transition from the background to the active state; here you can undo many of the changes made on entering the background.
    }

    // 여기까지 와야 화면이 뜸
    func applicationDidBecomeActive(_ application: UIApplication) {
    // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
    }


    // 더블클릭해서 날리는 것만 applicationWillTerminate, terminate에 대한 모든 경우를 보증하지는 않음
    func applicationWillTerminate(_ application: UIApplication) {
    // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
    }

}

{% endhighlight %}





### View Controller

{% highlight swift %}
class ViewController: UIViewController {

// viewWillAppear
// viewDidAppear
// viewWillLayoutSubviews
// viewDidDisappear
// viewDidLoad() -> 이후 viewDidAppear()가 눈에 보이는 시점

override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
    }

}
{% endhighlight %}
