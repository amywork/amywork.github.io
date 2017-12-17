---
layout: post
title: "GithubAPI"
author: "younari"
---

- [Fast Campus](http://www.fastcampus.co.kr/dev_camp_rxswift/) 수업에서 [강사님](https://github.com/intmain)이 API와 관련된 클래스들을 구조화 하는 방식이 너무 좋았어서, 포스팅을 올려봅니다 🙂👏🏻💖
- *이 프로젝트와 관련된 github API guide는 [여기](https://developer.github.com/v3/issues/#list-issues-for-a-repository) 에서 확인할 수 있습니다.*

# Spec
- **01. `repoIssues` / `issueComment`** : 깃헙 API를 call 해서 Owner의 Repo에 포스팅된 Issues (+이슈에 달린 comments)를 리스트 형태로 가져온다.
- **02. `createIssue` / `editIssue`** : Issue를 create 하거나 edit할 수 있다.
- **03. `createComment`** : 개별 Issue에 댓글을 달 수 있다.


# API :: Protocol
- API 콜과 관련된 기능들을 통제할 `protocol API` 가 있습니다.
- 여기에는 위의 SPEC을 구현하기 위한 메소드들이 정의되어 있습니다.

{% highlight swift %}

// 대략 아래와 같은 메소드들... 

internal func repoIssues(owner: String, repo: String) -> (Int, IssueResponsesHandler) -> Void
internal func createComment(owner: String, repo: String, number: Int, comment: String, completionHandler: (DataResponse<Model.Comment>) -> Void) -> <<error type>>
internal func createIssue(owner: String, repo: String, title: String, body: String, completionHandler: (DataResponse<Model.Issue>) -> Void) -> <<error type>>

{% endhighlight %}


# Github API :: Struct
- `API Protocol` 을 채택한 `Github API` 구조체입니다.
- 위 메소드들이 실제로 구현되어 있어, `response`를 받아 `Completion Handler`를 실행하는 녀석들입니다. 내부에 `API Call`은 `Github Router`의 `manager`를 통해 처리합니다.

{% highlight swift %}

internal struct GitHubAPI : API {
	
	func closeIssue(owner: String, repo: String, number: Int, issue: Model.Issue, completionHandler: @escaping (DataResponse<Model.Issue>) -> Void) {
	    var dict = issue.toDict
	    dict["state"] = Model.Issue.State.closed.rawValue
	    GitHubRouter.manager.request(GitHubRouter.editIssue(owner: owner, repo: repo, number: number, parameters: dict)).responseSwiftyJSON { (dataResponse: DataResponse<JSON>) in
	        let result = dataResponse.map({ (json: JSON) -> Model.Issue in
	            Model.Issue(json: json)
	        })
	        completionHandler(result)
	    }
	}
	
}

{% endhighlight %}


# Github Router :: Enum
- API Call을 처리하는 아이입니다.
- 여기서 효율적이라고 생각했던 점은, 이 부분이 enum으로 되어있어서, `switch self` 하면서 case마다 알맞은 `URLRequest`를 return 하게됩니다.
- `baseURL`
- `manager: Alamofire.SessionManager`
- `method: HTTPMethod`
- `path: String`
- `func asURLRequest() throws -> URLRequest`

{% highlight swift %}

enum GitHubRouter {
    case repoIssues(owner: String, repo: String, parameters: Parameters)
    case issueComment(owner: String, repo: String, number: Int, parameters: Parameters)
    case createComment(owner: String, repo: String, number: Int, parameters: Parameters)
    case editIssue(owner: String, repo: String, number: Int, parameters: Parameters)
    case createIssue(owner: String, repo: String, parameters: Parameters)
}

{% endhighlight %}

{% highlight swift %}

/*--base URL String--*/
static let baseURLString: String = "https://api.github.com"
   
/*--Responsible for creating and managing Request objects, as well as their underlying NSURLSession.--*/
static let manager: Alamofire.SessionManager = {
    let configuration = URLSessionConfiguration.default
    configuration.timeoutIntervalForRequest = 10
    configuration.timeoutIntervalForResource = 10
    configuration.urlCache = URLCache(memoryCapacity: 0, diskCapacity: 0, diskPath: nil)
    configuration.httpCookieStorage = HTTPCookieStorage.shared
    let manager = Alamofire.SessionManager(configuration: configuration)
    return manager
}()

/*--HTTP Method--*/
var method: HTTPMethod {
    switch self {
    case .repoIssues,
         .issueComment:
        return .get
    case .createComment,
         .createIssue:
        return .post
    case .editIssue:
        return .patch
    }
}
    
/*--URL Path--*/
var path: String {
    switch self {
    case let .repoIssues(owner, repo, _):
        return "/repos/\(owner)/\(repo)/issues"
    case let .issueComment(owner, repo, number, _):
        return "/repos/\(owner)/\(repo)/issues/\(number)/comments"
    case let .createComment(owner, repo, number, _):
        return "/repos/\(owner)/\(repo)/issues/\(number)/comments"
    case let .editIssue(owner, repo, number, _):
        return "/repos/\(owner)/\(repo)/issues/\(number)"
    case let .createIssue(owner, repo, _):
        return "/repos/\(owner)/\(repo)/issues"
    }
}

{% endhighlight %}
    
{% highlight swift %}

/*--URLRequest--*/
func asURLRequest() throws -> URLRequest {
    let url = try GitHubRouter.baseURLString.asURL()
    
    var urlRequest = URLRequest(url: url.appendingPathComponent(path))
    urlRequest.httpMethod = method.rawValue
    if let token = GlobalState.instance.token, !token.isEmpty {
        urlRequest.setValue("token \(token)", forHTTPHeaderField: "Authorization")
    }
    
    switch self {
    case let .repoIssues(_, _, parameters):
        urlRequest = try URLEncoding.default.encode(urlRequest, with: parameters)
    case let .issueComment(_, _, _, parameters):
        urlRequest = try URLEncoding.default.encode(urlRequest, with: parameters)
    case let .createComment(_, _, _, parameters):
        urlRequest = try JSONEncoding.default.encode(urlRequest, with: parameters)
    case let .editIssue(_, _, _, parameters):
        urlRequest = try JSONEncoding.default.encode(urlRequest, with: parameters)
    case let .createIssue(_, _, parameters):
        urlRequest = try JSONEncoding.default.encode(urlRequest, with: parameters)
    }
    
    return urlRequest
}

{% endhighlight %}
