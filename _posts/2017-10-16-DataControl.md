---
layout: post
title: "Data Control"
author: "younari"
---

# 서버와의 데이터 연동시 Data Control 
- **DB, 서버에서 전달 받은 데이터를 바로 사용하지 않고 Struct의 형태로 데이터 모델을 만들어서 사용한다.**

# 데이터모델을 구성하는 이유- **사용 편의성**: 서버로부터 복잡한 구조의 데이터를 받아 (ex.JSON 파일) Collection Type으로 갖고 있게 되는데, 매번 사용할 때마다 데이터를 끄집어 내기위해 Array나 Dictionary의 instance를 만들어야 하는 것은 불편하므로 Data Model을 만들어두는 것이 편하다.- **안정성**: Dictionary의 데이터는 key값을 통해 데이터에 접근 해야 하는데, String type인 key값은 다양한 곳에서 사용하게 되면 오타의 위험성이 커진다.- **수정 용의**: Dictionary의 key값이 변경되는 경우 코드 내 사용된 모든 곳을 찾아서 직접 바꿔줘야 하는 것은 번거로운 작업이므로, Data Model을 만들어 두고 해당 Data Model만 관리하면 중복 작업을 덜 수 있다.

# Sample Code
- User Model Structure 만들기
- ID,PWD,Email,Birthday,Gender로 구성된 User Data를 서버로부터 Dictionary 형태로 받았을 경우 해당 Dictionary를 UserModel Struct 형태로 변환해서 갖고 있도록 작업한다.
- 이후 앱 내에서 User Data를 활용해야 할 경우, UserModel의 인스턴스를 만들어 Dot 문법으로 접근하여 쉽게 사용할 수 있을 것이다.

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
    var birthday: String?
    var gender: Gender?
    
    init?(userDic:[String:Any]) {
        
        // Required Properties
        guard let userID = userDic["userID"] as? String else { return nil }
        self.userID = userID
        
        guard let userPWD = userDic["userPWD"] as? String else { return nil }
        self.userPWD = userPWD
        
        guard let email = userDic["email"] as? String else { return nil }
        self.email = email
        
        
        // Optional Properties
        self.birthday = userDic["birthday"] as? String
        
        if let gender = userDic["gender"] as? Int, (gender == 1 || gender == 2)
        {
            self.gender = Gender(rawValue: gender)
        }
        
    }
    
}

{% endhighlight %}
