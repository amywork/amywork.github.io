---
layout: post
title: "Singletone Pattern"
author: "younari"
---

### 파일을 저장하는 여러가지 방법에는 여러가지가 있다.
- **Property List**
- SQLite
- 아카이빙
- iOS DataBase(Core Data)- Network(Server DB)

### 이중에서 Singletone Design Pattern으로 데이터를 저장하는 방법 중에 하나인, Plist에 대해 알아보려고 한다.

# Singletone Design Pattern
- 데이터 처리에 대한 디자인 패턴
- 어플리케이션 전 영역에 걸쳐서 클래스에 대한 단 하나의 인스턴스만 존재해야 할 때 (App내에서 공유하는 단 1개의 객체) 싱글톤 디자인 패턴으로 설계한다.
- init을 통해 인스턴스를 내가 만들 수 없으며, 강제적으로 접근만 가능하게 만든다.
- 가령 Setting이나 NotificationCenter 정보는 Singletone으로 만드는 것이 좋다. 앱 전체에 적용되어야 하기 때문이다.
- 구현 방법: **Class에서 Static**을 통해 단 1개의 인스턴스만 만든다. 

{% highlight swift %}

class SingletonClass {
	// 최초로 접근할 때 한번만 인스턴스가 만들어짐
	static var sharedInstance: SingletonClass = SingletonClass()
	
	// init은 무조건 private으로 해야됨
	private init() { }
}

{% endhighlight %}



<br><hr><hr><br>

# Plist
- **Key,Value 구조 (딕셔너리 구조)**로 데이터를 저장하는 심플한 데이터 저장법
- 파일이 저장되는 곳: **Bundle 또는 Documents 폴더**

# Plist - Bundle
- App을 설치할 때 필요한 정보들이 저장되는 장소 
- 데이터를 불러오기만 가능함 (읽기 전용)
- **코드, 이미지, 사운드, nib 파일, 프레임 워크,설정파일 등 코드와 리소스가 모여있는 file system 내의 Directory**

### 👌🏻 01. App의 Main Bundle에 접근하기 - `let mainBundle = Bundle.main`

### 👌🏻 02. bundle에 있는 파일 Path 가져오기
- Bundle.main.path로 접근하여, NSDictionary 또는 NSArray 등으로 불러온다.

### 👌🏻 03. Path를 통해 객체로 변환, 데이터 불러오기

{% highlight swift %}
if let path = Bundle.main.path(forResource: "UserPlist", ofType: "plist"),
let dic = NSDictionary(contentsOfFile: path) as? [String:String] {
    let userDataModel = UserModel(userDic: dic)
}
{% endhighlight %}

{% highlight swift %}
if let imagePath = Bundle.main.path(forResource: "UserImage", ofType: "PNG") {
	let imgae = UIImage(contentsOfFile: imagePath)
}
{% endhighlight %}


# Plist - Documents
- App을 실행하면서 발생하는 데이터를 저장하는 장소
- 데이터를 저장하고 불러오기 모두 가능함 (쓰기, 읽기)

### 👌🏻 01. NSSearchPathForDirectoriesInDomains
- 일단 도큐멘트에 내가 원하는 plist 파일이 있는지 찾아본다.

{% highlight swift %}
let path:[String] =NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)
let basePath = path[0] + "/fileName.plist"
{% endhighlight %}

### 👌🏻 02. FileManager.default.fileExists(atPath:)
- Document folder에 파일이 없다면 Bundle에 있는 파일을 복사해준다.
- 혹은, Document folder에 파일이 없다면 새로운 plist 파일을 생성한다.

{% highlight swift %}
if !FileManager.default.fileExists(atPath: basePath) {
	if let fileUrl = Bundle.main.path(forResource: "fileName", ofType: "plist") {    do {         try FileManager.default.copyItem(atPath: fileUrl, toPath: basePath)    } catch  {         print("fail")	} 
	}}
{% endhighlight %}

{% highlight swift %}
if !FileManager.default.fileExists(atPath: basePath) {
	
	// dic은 내가 만든 어떠한 Dictionary라고 가정함
	
	let nsDic:NSDictionary =  NSDictionary(dictionary: dic)	nsDic.write(toFile: basePath, atomically: true)
	}
{% endhighlight %}

### 👌🏻 03. NSDictionary(contentsOfFile: basePath)
- document 폴더에 있는 파일을 NSDictionary을 통해서 Dictionary 인스턴스로 불러온다.

{% highlight swift %}
if let dict = NSDictionary(contentsOfFile: basePath) as? [String: AnyObject]{   // use swift dictionary as normal}
{% endhighlight %}


### 👌🏻 04. write(toFile)
- Dictionary 내용을 변경한 뒤에 덮어쓴다.

{% highlight swift %}

if let dict = NSDictionary(contentsOfFile: basePath) as? [String: AnyObject]{

	// dictionary를 수정   var loadData = dict
   loadData.updateValue("addData", forKey: "key")
	
	// dictionary를 NSDictionary로 변경 후 writeTofile 메소드를 통해 파일에 저장	let nsDic:NSDictionary = NSDictionary(dictionary: loadData)	nsDic.write(toFile: basePath, atomically: true)}

{% endhighlight %}


<br><hr><hr><br>


# User Defaults
- User Defaults는 클래스(NSObject)로 설계되어있다.
- 내부적으로 Plist 파일로 저장되며, 보안에 강하지 않다.
- Documents에 있을 확률이 높다.

## 👌🏻 User Defaults 활용법
- **저장하기:** UserDefaults.standard.set("ZICO", forKey: "userID")
- **읽기:** let aUser:String = UserDefaults.standard.object(forKey: "UserID") as! String

## 👌🏻 User Defaults 클래스(NSObject) 구조
- open class var standard: UserDefaults { get }- open func object(forKey defaultName: String) -> Any? 
- open func string(forKey defaultName: String) -> String? 
- open func array(forKey defaultName: String) -> [Any]?- open func set(_ value: Any?, forKey defaultName: String)- open func removeObject(forKey defaultName: String)


<br><hr><hr><br>


### (참고) SandBox
- iOS는 APP을 SandBox 구조로 관리
- iOS App은 개별적인 단위로 외부에는 폐쇄적인 단 하나의 폴더를 가지고 있다.
- 안전성: 해킹이 불가함