---
layout: post
title: "RxSwift 강의 리뷰"
author: "younari"
---

<br>

오늘의 포스팅은 수강 만족도가 정말 높았던 **[Fast Campus](http://www.fastcampus.co.kr)** 의  **[RxSwift](http://www.fastcampus.co.kr/dev_camp_rxswift/)** 강의 리뷰입니다. 

- **강의 기본 정보** : [Fast Campus RxSwift](http://www.fastcampus.co.kr/dev_camp_rxswift/)
- **강의 시간** : 매주 토요일 5시간, 8주 과정
- **강사님 Github Profile** : [/intmain](https://github.com/intmain)
- **RxSwift Github** : [ReactiveX / RxSwift](https://github.com/ReactiveX/RxSwift)

<br>

![RxSwift Code](https://younari.github.io/images/RxSwift2.png)


# 이 강의가 나와 맞을까?

아마 이 강의 수강을 고민하시는 분이라면, 

- 1️⃣ iOS 개발 경력을 갖고 계신 분 
- 2️⃣ 스위프트를 배운지 얼마 안된 분 
- 3️⃣ 안드로이드 개발을 하시던 분 
- 4️⃣ 앱 개발에 처음 입문하시는 분 

등등 다양한 백그라운드의 **개발자** 분들이실텐데요.

이 강의는 초급자를 대상으로 하는 강의라기 보다, Swift 언어로 iOS 개발에 어느 정도 익숙한 상태에서 **점프업을 원하시는 분**에게 최적화된 강의라는 생각이 듭니다. 즉 1️⃣ 번 분들에게 최적화된 강의라고 생각합니다.  

안드로이드 개발을 하셨던 분들(3️⃣)이라도 스위프트 문법이나 Xcode(특히 스토리보드, xib) 사용이 익숙하지 않다면 그 부분을 해결하고 오지 않으시면 수업시간에 그냥 청강이 될 수 있을 것 같구요.

앱 개발 자체에 처음 입문하시는 분(4️⃣)이라면 이 강의보다는 [iOS 입문 강의](http://www.fastcampus.co.kr/dev_camp_swift4/)를 먼저 수강하시는 게 맞을 것 같습니다. 혹은 입문 강의를 듣고 있는 상태에서 이 강의를 같이 병행하시는 것도 추천합니다.

저는 비전공자 출신으로, 온라인 커머스 기획일을 하다가, 개발로 전향하여 2017년 9월부터 시작하는 iOS 개발 스쿨을 2개월 정도 듣고 있는 상태에서 2017년 10월 말에 RxSwift 강의를 병행하였습니다. 즉 저는 스위프트를 배운지 2개월 밖에 되지 않은, 2️⃣번에 해당되는 수강생이었는데요. 

2년 넘게 IT 서비스 기획 업무를 하면서, 개발에 대한 지식이 아주 없는 편은 아니었지만, 스위프트를 첫 언어로 개발을 시작한지 얼마 안 된 상황이었기 때문에 강의 내용이 처음에는 조금 버거웠습니다.

그렇지만 조금 욕심을 부린다면, 강의를 마치고 났을 때 폭풍 성장을 경험할 수 있었기 때문에 2️⃣번 분들에게도 이 강의를 추천합니다. 

![RxSwift Main Project](https://younari.github.io/images/RxSwift1.png)

# 핵심 프로젝트

**[RxSwift](http://www.fastcampus.co.kr/dev_camp_rxswift/)** 강의에서 8주동안 진행하는 핵심 프로젝트는 **깃헙 Repo Issue 앱 만들기** 입니다. **Github**에서 제공하는 **[List issues for a repository API](https://developer.github.com/v3/issues/#list-issues-for-a-repository)**를 콜해서 특정 Repo를 입력하면, 해당 Repo에 포스팅된 Issue들을 콜렉션뷰에 뿌려줍니다. 직접 **Issue** 를 **create / edit** 할 수도 있고, **comment** 를 작성할 수도 있습니다.

### ✔︎ Spec
- **01. `repoIssues` / `issueComment`** : 깃헙 API를 call 해서 특정 Owner의 Repo에 포스팅된 Issues (+이슈에 달린 comments)를 리스트 형태로 가져온다.
- **02. `createIssue` / `editIssue`** : Issue를 create 하거나 edit할 수 있다.
- **03. `createComment`** : 개별 Issue에 댓글을 달 수 있다.

# 핵심 프로젝트 리뷰

**[RxSwift](http://www.fastcampus.co.kr/dev_camp_rxswift/)** 패스트캠퍼스 커리큘럼에 따르면, 총 8주 과정에서 4주동안은 위 프로젝트를 Swift로 작성하고, 남은 4주 동안은 RxSwift로 포팅하게됩니다. 하지만 실제로는 Swift로 구현하는데 3주정도 소요됐고, Rx로 포팅하는데 2.5주 정도 소요되었습니다. 나머지는 2.5주는 RxSwift 이론을 배우거나, RxSwift를 이용한 다양한 샘플 프로젝트를 진행하는 데 사용되었습니다.

## 1-4주차 리뷰

저는 이 프로젝트 자체가 정말 **현업에 응용할 수 있는 모든 것**을 담고 있는 프로젝트였던 것 같아서 너무너무 좋았습니다. 강사님이 커리큘럼으로 이 프로젝트를 구성하신 것이 정말 신의 한수인 것 같아요. 👏🏻 실무도 바쁘실텐데, 매 주 이렇게 고퀄리티의 코드 파일들을 직접 준비해주셨다는 것이 놀랍기도 하고 감사한 부분입니다.

RxSwift로 넘어가기 전인 4주 동안에도, 강사님의 실무 경험이 묻어나는 노하우들이 담긴 코드를 전수받으면서 저는 코드 구조화에 있어서 정말 몇 단계 점프업 할 수 있었습니다. *"이렇게 추상화를 하는구나!", "xib를 이렇게 다양하게 활용할 수 있었구나", "protocol이 이래서 필요하구나"* 등등.. 특히 수업에서 배운 프로토콜과 제너릭, xib 활용법들은 제가 진행중인 개인 프로젝트에도 폭풍 응용하고 있습니다.

하지만 누군가에게 장점으로 느껴지는 것이 누군가에겐 단점일 수도 있을 것 같습니다. 만약 스위프트로 iOS 개발을 이미 오래 하셨고, 함수형 프로그래밍과 RxSwift 학습만이 유일한 목적이었다면 초반 3주차 까지는 복습하는 시간이었을 수도 있었을 것 같습니다. 

> [수업 필기 Sample](https://younari.github.io/2017-12-14/RxRGBSlider)

### ✔︎ 좋았던 점 

- 데이터 모델링
- **OAuth** 인증
- **Real github api** 콜을 통한 네트워크 통신
- **API protocol** 화
- **ViewController** 추상화
- **Custom View** :: **xib**로 **collectionViewCell** 및 **reusableView** 구현
- **AutoLayout**, **CollectionView**, **TableView**
- **TableView Dynamic Cell Size Height**
- **closure**, **enum**, **Generic** 등 스위프트 고급 문법 응용

### ✔︎ 1-4주차 사용 Cocoapod

- `pod 'Alamofire'`
- `pod 'AlamofireImage'`
- `pod 'SwiftyJSON'`
- `pod 'OAuthSwift'`

<br>

## 5-8주차 리뷰

4주차 부터는 본격적인 Rx 수업이었는데요. RxSwift로 들어오면서 Observable이라는 개념을 익히고, 객체지향에서 handler로 비동기 문제를 해결하던 것들이 Observable - Subscribe 를 통해 스트리밍(!)되는 것처럼 작동하는 것이 신기하기도 하고 효율적으로 느껴졌습니다.

### ✔︎ 좋았던 점 

- **Rx**를 이용한 **CollectionView** 관리
- **map과 flatMap**을 이용한 **이벤트 체이닝**

### ✔︎ 5-8주차 사용 Cocoapod

- `pod 'Alamofire'`
- `pod 'AlamofireImage'`
- `pod 'SwiftyJSON'`
- `pod 'OAuthSwift'`
- `pod 'RxSwift'`
- `pod 'RxCocoa'`
- `pod 'RxDataSources'`
- `pod 'RxKeyboard'`

### ✔︎ 아쉬웠던 점
강사님께서도 말씀하셨지만, RxSwift로 이 프로젝트를 포팅하기 이전에 observable과 더 친해지는 시간을 갖기 위해 다양한 Rx 샘플 프로젝트들을 먼저 다뤄보는 것이 더 수강생 친화적일 것 같습니다. 

<br>

# 서브 프로젝트 리뷰

RxSwift와 친해지는 계기를 마련하기 위해, 강사님께서 다양한 예제들을 준비해주셨습니다. 이 부분을 메인 프로젝트를 진행하기 이전인 4~5주차에 했었다면 좀 더 좋았을텐데, 6주차 정도에 진행했어서 조금 아쉬움이 있습니다. 확실히 요런 미니 프로젝트들이 간단하기도 하고 재밌기도 훨씬 재밌습니다. (ㅋㅋ)

- 기본적인 문법 
- Rx 구구단 만들기
- RGB 슬라이더 만들기
- Rx로 애니메이션 구현하기
- Rx AlertController
- Rx 이미지 피커 + 프로그레스 바
- Rx Url request

<br>

# Reactor Kit

강좌 마지막 시간에는 스위프트계의 젊은 천재 수열님이 만들어주신 **[ReactorKit](https://github.com/ReactorKit/ReactorKit)** 을 잠깐 맛보는 시간을 가졌습니다. 뷰는 UI처리만 하게 하고, 비즈니스 로직들을 담은 모델은 Reactor가 감싸게끔 설계하는 방식입니다. 실제로 ReactorKit은 강사님이 근무하고 계신 Kaka..🤧에서도 몇몇 프로젝트에 사용하고 계시다고 합니다. 😁


> [Reactor Kit 강의노트](https://younari.github.io/2017-12-16/ReactorKit)

### ✔︎ Reactor Kit?
- UI와 비즈니스 로직을 분리하는 것
- View는 Action을 emit한다.
- Reactor는 State를 emit한다.

<br>

# 이론 vs 실습
- 이론과 실습이라는 두 축이 있다면, 이론보다는 실습 중심입니다.
- 저는 이론을 매우 싫어하는 사람이어서 실용적인 실습 중심의 강의가 좋았습니다.
- 옵저버, 옵저버블, 섭스크라이버 등등 이런 개념을 구체적으로 이해하는 것보다 내가 지금 당장 api를 콜해서 리스폰스를 받아서 콜렉션뷰와 여러곳을 리로드 해야하는데 Rx를 쓰면 어떻게 더 간단하게 할 수 있는거지? 가 더 궁금했었기 때문입니다.

```
doneButton.rx.tap
            .flatMap { [weak self] _ -> Observable<Model.Issue> in
                guard let `self` = self else { return Observable.empty() }
                let title = self.titleTextField.text ?? ""
                let body = self.bodyTextView.text ?? ""
                return App.api.postIssue(owner: GlobalState.instance.owner, repo: GlobalState.instance.repo, title: title, body: body)
            }.map { _ -> Void in
                return ()
            }.do(onNext: { [weak self] _ in
                self?.dismiss(animated: true, completion: nil)
                }, onError: { error in
                    
            }).bind(to: reloadSubject!).disposed(by: disposeBag)
```

<br>

# 수업 듣기 전에 보면 좋아요
- [rxmarbles.com](http://rxmarbles.com)
- [Introducing RxSwift](https://www.raywenderlich.com/158026/introducing-rxswift-reactive-programming-swift)

<br>
# 요약

### ✔︎ 수업 자료
- 모든 코드를 깃으로 관리해서 올려주셨습니다. 수업 중간에 내용을 놓치더라도  헤드를 바꿔가면서 코드를 리뷰해볼 수 있습니다.

### ✔︎ 수업 방식
- 강사님이 코드에 빈칸을 뚫어주셔서, 해당 브랜치를 pull하여 수업시간에 수업을 들으며 blank 채우기를 하며 수업을 따라갈 수 있었습니다. 
- 가장 이상적인 것은, 수업 시작 전에 직접 코드를 자기가 생각해서 짜와보고, 강사님 것과 비교해보고, 다시 수정하는 것일 것 같지만...
- 저는 수업을 일단 듣고 나서, 다시 뉴 프로젝트를 만들어서 백지에서 제가 다시 짜보는 식으로 복습했습니다.

### ✔︎ 질문에 대한 답변
- 실무진이셔서 매우 바쁘실텐데도, 강사님께서 **페이스북 그룹**을 통해 늦은 밤에도 정말 정성껏 답변을 해주셨습니다. 저의 궁금증을 정확하고 자세하게 설명해주셨습니다. (감동..😢💕)


### ✔︎ 수업 인원
- 10명 조금 넘는 인원입니다. 혼자 여자 수강생이었어서 외로웠습니다 😢

### ✔︎ 주관적 난이도
- ⭐️⭐️⭐️⭐️⭐️
- 시간을 투자한 만큼 성장하는 커리큘럼입니다. 공부도 때가 있다고... 공부 하고 싶을 때 돈 아끼지 말자는 것이 저의 가치관입니다. (ㅋㅋ) 

### ✔︎ 주의 사항
- 토요일마다 5시간씩 쉬는시간 거의 없이 빡세게 진행되었습니다. 중간 중간 배가 많이 고픕니당. 먹을 것을 꼭 챙겨가세요. 🍰🍫 


# 맺음말
- 스위프트의 매력은 멀티패러다임 랭귀지 라는 것 아닐까요? 객체 지향으로만 코딩하셨던 분들이라면 RxSwift와 꼭 한 번 인연을 맺어보시길 추천합니다. 강사님께서 워낙 실력도 빵빵하시고 프로젝트 준비도 너무 잘해주셔서 코딩 스킬을 점프업 하는데 아주 좋은 강의인 것 같습니다. 🙃👍🏻 

