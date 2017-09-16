---
layout: post
title: "iOS Design Pattern"
author: "younari"
---

## iOS Design Pattern 
- [The Role of View Controllers](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457)
- [View Controller Programming Guide for iOS](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/DefiningYourSubclass.html#//apple_ref/doc/uid/TP40007457-CH7-SW1)
- [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)
- [Document-Based Applications in iOS](https://developer.apple.com/library/content/documentation/DataManagement/Conceptual/DocumentBasedAppPGiOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011149)
- [The View Controller Hierarchy](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/TheViewControllerHierarchy.html#//apple_ref/doc/uid/TP40007457-CH33-SW1)

## MVC model
- **Model**: What your application is (not how it displays)
- **Controler**: How it displays.
- **View**: Controller's minions, generic things (like UI Buttons)
- It's all about managing communication between camps.

### Relationship beteween camps.
- Controllers can always talk directly to their Model.
- Outlet: Controllers can also talk directly to their View. 
- The Model and View should never speak to each other. 
- The Model is UI-independent.

### Can the View speack to its Controller? 
- Sort of.
- The Controller can drop a target on itself. (IBAction)
- Then hand out an action to the view.
- The View sends the action.
- The Controller sets itself as the View's delegate.
- The delegate is set via a protocol.

### View do not own the data they display.
- They ask for data source to Controller.
- Controllers interpret/format Model information for the View.

### Models are UI Independent.
- They do not directly speack to Controller.
- They are like Radio Station.
- Controllers tune in to interesting stuff.

### MVCs working together.


## iOS MVC model (by apple)
- The first thing to notice is that iOS apps use a model-view-controller architecture. This pattern separates the app’s data and business logic from the visual presentation of that data. 
- What distinguishes one iOS app from another is the data it manages (and the corresponding business logic) and how it presents that data to the user. Most interactions with UIKit objects do not define your app but help you to refine its behavior. For example, the methods of your app delegate let you know when the app is changing states so that your custom code can respond appropriately.

### UI Application object
- At the heart of every iOS app is the UIApplication object, whose job is to facilitate the interactions between the system and other objects in the app.
- The UIApplication object manages the event loop and other high-level app behaviors. It also reports key app transitions and some special events (such as incoming push notifications) to its delegate, which is a custom object you define. Use the UIApplication object as is—that is, without subclassing.
- 사용자 클릭 -> OS -> UI Application -> .. -> View -> View Controller
- Event loop : Que 구조(선입선출)

### App delegate object
- UIApplicationMain이 AppDelegate를 호출
- The app delegate is the heart of your custom code. This object works in tandem with the UIApplication object to handle app initialization, state transitions, and many high-level app events. This object is also the only one guaranteed to be present in every app, so it is often used to set up the app’s initial data structures.
- 원래 순서는 delegate -> viewController 이동인데 스토리보드라는 변수

### Documents and data model objects
- Data model objects store your app’s content and are specific to your app. For example, a banking app might store a database containing financial transactions, whereas a painting app might store an image object or even the sequence of drawing commands that led to the creation of that image. (In the latter case, an image object is still a data object because it is just a container for the image data.)
- Apps can also use document objects (custom subclasses of UIDocument) to manage some or all of their data model objects. Document objects are not required but o er a convenient way to group data that belongs in a single file or file package. For more information about documents, see Document-Based App Programming Guide for iOS.

### View controller objects
- View controller objects manage the presentation of your app’s content on screen. A view controller manages a single view and its collection of subviews. When presented, the view controller makes its views visible by installing them in the app’s window.
- The UIViewController class is the base class for all view controller objects. It provides default functionality for loading views, presenting them, rotating them in response to device rotations, and several other standard system behaviors. UIKit and other frameworks define additional view controller classes to implement standard system interfaces such as the image picker, tab bar interface, and navigation interface.


### UI Window object
- A UIWindow object coordinates the presentation of one or more views on a screen. Most apps have only one window, which presents content on the main screen, but apps may have an additional window for content displayed on an external display.
- To change the content of your app, you use a view controller to change the views displayed in the corresponding window. You never replace the window itself.
- In addition to hosting views, windows work with the UIApplication object to deliver events to your views and view controllers.


### View obejects, control objects, layer objects
- Views and controls provide the visual representation of your app’s content. A view is an object that draws content in a designated rectangular area and responds to events within that area. Controls are a specialized type of view responsible for implementing familiar interface objects such as buttons, text fields, and toggle switches

- The UIKit framework provides standard views for presenting many di erent types of content. You can also define your own custom views by subclassing UIView (or its descendants) directly.

- In addition to incorporating views and controls, apps can also incorporate Core Animation layers into their view and control hierarchies. Layer objects are actually data objects that represent visual content. Views use layer objects intensively behind the scenes to render their content. You can also add custom layer objects to your interface to implement complex animations and other types of sophisticated visual e ects.

### App delegate
{% highlight swift %}

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.
    return true
    }

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
