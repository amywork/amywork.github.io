---
layout: post
title: "iOS Newtwork system"
author: "Amy"
---

- **기초 지식**: [컴퓨터 공학 용어 사전](https://amywork.github.io/2017-09-05/Programming)
- **공식 문서**: [URL Loading System](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

# URL Load System
- iOS에서는 URL을 통해서 컨텐츠의 리소스를 받아올수 있는 가장 일반적인 방법으로 URLSession을 사용한다. 
- **URL Loading, URL Authentification, URL Credentials ...**
- 우리가 집중할 부분은 URL Loading 중에, URLSession과 request, response 부분!

## URL Loading
- **URL Session** : 서버와의 연결을 담당하는 아이
- **URL Request** : 요청에 대한 정보를 담고 있는 구조체 (URL, 호스트 정보, 헤더 파일, 바디 정보, HTTP 메소드 등을 캡슐화하고 있다. 데이터 뭉치!)
- **URL Response** :
- cf. objc의 mutable은 swift의 var와 같음
- cf. objc의 request는 class이나 swift는 struct.


### ✔️ URL Session
- URL Loading System에서 컨텐츠를 검색하는 가장 일반적이고 간단한 방법이다.
- URLSession은 **HTTP requests**를 통해 데이터를 보낼 수도 있으며, 그냥 **URL**을 통해서도 보낼 수 있다.
- APP이 실행되지 않은 상태에서도 **백그라운드에서 Upload 및 Download 기능**을 제공한다.
- File Transfer Protocol `(ftp://)`
- Hypertext Transfer Protocol `(http://)`
- Hypertext Transfer Protocol with encryption `(https://)`
- Local file URLs `(file:///)`
- Data URLs `(data://)`

#### 사용법
- **싱글톤**으로 존재하기 때문에 **shared**로 인스턴스 불러온다.
- init으로 직접 만들 수도 있다. 
- init(configuration: URLSessionConfiguration)
- dataTask, uploadTask, downloadTask 메소드를 통해 **URLSessionDataTask**를 만들어낸다.
- 하나의 테스크는 하나의 응답을 받는다.
- 응답에 대해서 클로저, 델리게이트 메소드 등으로 처리한다.

#### URLSessionTask
- URLSessionTask는 URLSession의 작업 하나(Task)를 나타내는 추상클래스로 URLSession을 통해서만 생성 가능하다.
- URLSessionDataTask
- URLSessionUploadTask
- URLSessionDownloadTask

#### URLSessionConfiguration
- Session의 설정에 관련된 클래스
- 타임아웃, 캐시 정책등의 프로퍼티를 설정
- **default**: 디폴트 configuration 객체는 디폴드 값으로는 파일로 다운로드 될 때를 제외하고는 **Disk에 캐쉬를 저장**하며, 키체인에 자격을 저장한다.
- **ephemeral**: session 관련 데이터가 **메모리**에 올라간다.
- **background**: Session이 백그라운드에서 다운로드 작업과 업로드 작업을 마저 수행할 수 있도록 한다.


### ✔️ URL Request
- **URL**, **Cache Policy**, **timeoutInterval**과 함께 **init**

```
public init(url: URL, 
cachePolicy: URLRequest.CachePolicy = default, 
timeoutInterval: TimeInterval = default)
```

- 추가적으로 수정할 수 있는 **프로퍼티 및 메소드**
- httpMethod, httpBody, cachePolicy 등

```var cachePolicy: URLRequest.CachePolicyvar timeoutInterval: TimeIntervalvar networkServiceType: URLRequest.NetworkServiceTypevar httpMethod: String?var httpBody: Data?func addValue(_ value: String, forHTTPHeaderField field: String)func setValue(_ value: String?, forHTTPHeaderField field: String)
```
