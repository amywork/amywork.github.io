---
layout: post
title: "UserDefaults 유사 싱글톤 객체 만들기"
author: "younari"
---

# Singleton 디자인 패턴 실습
## UserDefaults와 유사한 싱글톤 객체, DataCenter 만들기

### Singletone Design Pattern
- 어플리케이션 전 영역에 걸쳐서 **클래스에 대한 단 하나의 인스턴스만** 존재해야 할 때 (App내에서 공유하는 단 1개의 객체가 되어야 할 때) 싱글톤 디자인 패턴으로 설계한다.
- init을 통해 인스턴스를 직접 만들 수는 없으며, 강제적으로 **접근만 가능하게** 만든다.

# 👌🏻 01. User Model Struct 설계
- ID, PWD, Email, BirthDay, Gender를 갖고 있는 모델 설계
- 같은 항목의 딕셔너리를 담고 있는 UserPlist 파일 생성

{% highlight swift %}
import Foundation
struct UserModel {

    enum Gender: Int {
        case women = 1
        case men = 2
    }
    
    var userID: String
    var userPWD: String
    var email: String
    var birthday: String
    var gender: Gender?
    var dictionary:[String:Any] {
        let genderValue = self.gender?.rawValue ?? 0
        let dic:[String:Any] = ["userID":self.userID, "userPWD":self.userPWD, "email":self.email, "gender":genderValue]
        return dic
    }
    
    init?(userDic:[String:Any]) {
        
        // Required Properties
        guard let userID = userDic["userID"] as? String else { return nil }
        self.userID = userID
        
        guard let userPWD = userDic["userPWD"] as? String else { return nil }
        self.userPWD = userPWD
        
        guard let email = userDic["email"] as? String else { return nil }
        self.email = email
        
        guard let birthday = userDic["birthday"] as? String else { return nil }
        self.birthday = birthday
        
        // Optional Properties
        if let gender = userDic["gender"] as? Int, (gender == 1 || gender == 2)
        {
            self.gender = Gender(rawValue: gender)
        }
        
    }
    
}
{% endhighlight %}

# 👌🏻 02. Data Center 설계하기
- Singleton 객체, DataCenter: UserModel과 데이터 UserPlist의 중개자 역할
- `static var main`
- `private init()`

{% highlight swift %}
import Foundation
class DataCenter
{
    // Singleton Instance (shared Instance)
    static var main: DataCenter = DataCenter()
    var currentUser: UserModel?
    
    private init() {
        loadUserData()
    }
    
    func object(forkey: String) -> Any? {
        let documentPath = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)[0] + "/UserPlist.plist"
        guard let documentDic = NSDictionary(contentsOfFile: documentPath) as? [String:Any] else { return nil }
        return documentDic[forkey]
    }
    
    // 최초에는 documentPath에 plist 파일이 없으므로 currentUser가 nil인 상태
    // 다시 로그인 할 떄는 AppDelegate에서 Background로 진입할 때 writeUserData() 하면서 documentPath에 plist 파일을 생성하므로, init할 때 loadUserData()를 하면서 currentUser가 생김
    
    func loadUserData() {
        // document Path는 String의 array로 들어오기 때문에, [0] + 내 파일명
        let documentPath = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)[0] + "/UserPlist.plist"
        guard let documentDic = NSDictionary(contentsOfFile: documentPath) as? [String:Any] else { return }
        currentUser = UserModel(userDic: documentDic)
    }
    
    func writeUserData() -> Bool {
        let documentPath = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)[0] + "/UserPlist.plist"
        guard let data = currentUser else { return false }
        let newDic = data.dictionary
        let NSDic = NSDictionary(dictionary: newDic)
        NSDic.write(toFile: documentPath, atomically: true)
        return true
    }
}
{% endhighlight %}

# 👌🏻 03. ViewController에서 DataCenter 사용
- `viewDidLoad()`에서 DataCenter의 currentuser 존재 여부에 따라 로그인 화면 or 메인 화면을 띄울 것을 결정한다.

{% highlight swift %}
import UIKit
class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // DataCenter
        if let _ = DataCenter.main.currentUser {
            // main 화면을 띄운다.
        }else {
            // 로그인 화면을 띄운다.
            // 사용자가 최초 로그인 -> 로그인 정보를 newUserDic으로 만들어서
            let newUserDic = ["userID":"","userPWD":"","email":""]
            DataCenter.main.currentUser = UserModel(userDic: newUserDic)
        }
    }

}
{% endhighlight %}


# 👌🏻 04. Write Data
- AppDelegate에서 Background로 진입할 때 writeUserData()를 통해 documentPath에 plist 파일 생성 (currentUser != nil)

{% highlight swift %}
func applicationDidEnterBackground(_ application: UIApplication) {
    // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
    // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
    
    let isSaved = DataCenter.main.writeUserData()
    print(isSaved)
}
{% endhighlight %}