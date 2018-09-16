---
layout: post
title: "Singletone Pattern"
author: "Amy"
---

# Read & Write Data

### 파일을 저장하는 방법에는 아래와 같이 여러가지가 있다.
- **Property List**
- SQLite
- 아카이빙
- iOS DataBase(Core Data)- Network(Server DB)

### 이중에서도 Singletone Design Pattern으로 데이터를 저장하는 방법 중에 하나인, Plist에 대해 알아보려고 한다.

# Singletone Design Pattern?
- 데이터 처리에 대한 디자인 패턴이다.
- 어플리케이션 전 영역에 걸쳐서 **클래스에 대한 단 하나의 인스턴스만** 존재해야 할 때 (App내에서 공유하는 단 1개의 객체가 되어야 할 때) 싱글톤 디자인 패턴으로 설계한다.
- init을 통해 인스턴스를 직접 만들 수는 없으며, 강제적으로 **접근만 가능하게** 만든다.
- 가령 Setting이나 NotificationCenter와 같은 객체는 Singletone으로 만드는 것이 좋다. 이와 같은 디폴트성이 강한 정보는 앱 전체에 적용되어야 하기 때문이다.
- 구현 방법: **Class에서 Static**을 통해 구현한다. 

## Singletone Design Pattern의 예시
- 스크린 정보를 가지고 있는 객체- `let screen = UIScreen.main`
- 어플리케이션 객체
- `let app = UIApplication.shared`
- 사용자 정보를 저장하는 객체- `let data = UserDefaults.standard` - 파일 시스템 정보를 가지고 있는 객체- `let fileManager = FileManager.default`

{% highlight swift %}

class SingletonClass {

	// 최초로 접근할 때 한번만 인스턴스를 만들어냄
	static var sharedInstance: SingletonClass = SingletonClass()
	
	// init은 무조건 private으로!
	private init() { }
}

{% endhighlight %}

<br>
<hr>
<br>

# Plist
- **Key,Value 구조 (딕셔너리 구조)**로 데이터를 저장하는 심플한! 데이터 저장법
- 파일이 저장되는 곳: **Bundle 또는 Documents 폴더**

# Bundle
- App을 설치할 때 필요한 정보들이 저장되는 장소 
- 데이터를 불러오기만 가능함 (읽기 전용)
- **코드, 이미지, 사운드, nib 파일, 프레임 워크, 설정 파일 등 핵심 코드와 리소스가 모여 있는 file system 내의 Directory**

### 👌🏻 01. App의 Main Bundle에 접근하기 - `let mainBundle = Bundle.main`

### 👌🏻 02. bundle에 있는 파일 Path 가져오기
- `Bundle.main.path(forResource:, ofType:)`

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


# Documents
- App을 실행하면서 발생하는 데이터를 저장하는 장소
- 데이터를 저장하고 불러오기 모두 가능함 (쓰기, 읽기)

### 👌🏻 01. NSSearchPathForDirectoriesInDomains
- 도큐멘트에 내가 원하는 plist 파일이 있는지 찾아본다.

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
	
	// dictionary를 NSDictionary로 변경 후 writeTofile 메소드를 통해 파일에 저장	
	let nsDic:NSDictionary = NSDictionary(dictionary: loadData)	nsDic.write(toFile: basePath, atomically: true)}

{% endhighlight %}


<br>
<hr>
<br>


# User Defaults
- User Defaults는 클래스(NSObject)로 설계되어있다. (싱글톤 오브젝트)
- 내부적으로 Plist 파일로 저장되며, 보안에 강하지 않다.
- Documents에 있을 확률이 높다.

## 👌🏻 User Defaults 활용법
- **저장하기:** UserDefaults.standard.set("ZICO", forKey: "userID")
- **읽기:** let user:String = UserDefaults.standard.object(forKey: "UserID") as! String

## 👌🏻 User Defaults 클래스(NSObject) 구조
- open class var standard: UserDefaults { get }- open func object(forKey defaultName: String) -> Any? 
- open func string(forKey defaultName: String) -> String? 
- open func array(forKey defaultName: String) -> [Any]?- open func set(_ value: Any?, forKey defaultName: String)- open func removeObject(forKey defaultName: String)


<br>
<hr>
<br>


### (참고) SandBox
- iOS는 APP을 SandBox 구조로 관리
- iOS App은 개별적인 단위로 외부에는 폐쇄적인 단 하나의 폴더로 관리되어 안전성이 있다. (해킹불가)