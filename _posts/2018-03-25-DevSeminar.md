---
layout: post
title: "Dev Seminar 2018"
author: "younari"
---

# Dev Seminar, 2018 리뷰

- **일시**: 2018년 3월 25일 일요일, 10시 ~ 19시 (9시간)
- **장소**: 역삼 마루 180


<br>

# RxSwift
### 키워드
- `비동기 처리`, `반응형 프로그래밍`
- `observable`, `binding`, `subscribe`
- `반응형 프로그래밍`은 `약속`이다. 내가 때마다 계속 말하지 않아도 (내가 매줄 코딩하지 않아도) 이런 이벤트가 발생되는 상황에서는 이렇게 하자고 "약속"한 것에 대해 제대로 반응해주는 것!
- **강사님의 RxSwift와의 첫 만남**: 카테고리 API 와 리스트 API → 두 가지 API 콜을 조인해서 써야했던 경우에 Rx를 도입하게 됨

### 실습
- [실습](https://github.com/intmain/RxColor)


```
// 가장 인상 깊었던 코드 조각...🙏🏻
func bind() {
        addButton.rx.tap.asObservable()
            .flatMap { [weak self] _ in
                ColorViewController.rx.create(parent: self)
                    .flatMap({ (cv: ColorViewController) -> Observable<UIColor> in
                        cv.rx.selectedColor
                    }).take(1) //take(1) 하면 observable이 complete 됨
            }.subscribe(onNext: { [weak self] (color: UIColor) in
                guard let `self` = self else { return }
                var colors = self.dataSource.value
                colors.append(color)
                self.dataSource.accept(colors)
            }).disposed(by: disposeBag)
        
        dataSource.bind(to: self.collectionView.rx.items(cellIdentifier: "colorCell", cellType: UICollectionViewCell.self)) { (index, color, cell) in
            cell.contentView.backgroundColor = color
            }.disposed(by: disposeBag)
        
        collectionView.rx.itemSelected.asObservable()
            .subscribe(onNext: { [weak self] (indexPath) in
                guard let `self` = self else { return }
                var colors = self.dataSource.value
                colors.remove(at: indexPath.item)
                self.dataSource.accept(colors)
            }).disposed(by: disposeBag)
        
    }
```


<br>

# 오토레이아웃 
### 오토레이아웃 디버깅

#### 🧐 View 에 이름 붙여서 찾아내기
- ✳︎ `Identity Inspector` 에서 `Accessibility`에 `Identifier`를 설정해놓으면 오토레이아웃 오류로 인해 디버깅 될 때 _**해당 아이덴티파이어 이름을 달고 디버거 콘솔에 출력**_ 된다.

#### 🧐 레이아웃 비주얼 디버거에서 포인터 주소로 검색하기
- ✳︎ Layout Debugger에서 포인터 주소로 뷰나 NSLayoutConstraint를 검색할 수 있다.

#### 🧐 symbolic breakpoint로 오토레이아웃 오류 시점에 앱 죽이기
- ✳︎ **`symbolic breakpoint`를 활용한 오토레이아웃 디버깅**: 브레이크포인트의 심볼로 `UIViewAlertForUnsatisfiableConstraints` 를 추가하면 해당 시점에서 브레이크포인트가 걸린다. 이 때 아래 방식으로 lldb 고고!


##### `po $r15`
```
< UIView: 0x7f910ef169f0; frame = (0 0; 375 812); autoresize = W+H; layer = <CALayer: 0x60400022e2e0>>
```

##### `po $rbx`
```
< __NSArrayM 0x600000252510>(
< NSLayoutConstraint:0x60400028a5a0 UIView:0x7f910ec003d0.width == 150   (active)>,
< NSAutoresizingMaskLayoutConstraint:0x60000028f3c0 h=--& v=--& UIView:0x7f910ec003d0.width == 100   (active)>
)
```

##### `po $r14`
##### `po [[UIWindow KeyWindow] autolayoutTrace]`

### 오토레이아웃 오픈소스
- [SnapKit](https://github.com/SnapKit/SnapKit)

### 실습
- [실습](https://github.com/younari/DevSeminar/tree/master/02_AutoLayout)


<br>

# TDD

### Test Coverage
- 전체 유효한 코드 대비 테스트가 커버한 비율

### 목표
- ViewController에 대해 테스트 비율을 100%로 만드는 것
- **Given**: 주어진 상황에서
- **When**: 이러한 조건일 때 실행된다면
- **Then**: 성공하는가 실패하는가 (검증)

### 단축키
- `cmd U`
- `cmd shift U`
- `cmd shift ,` 스키마 설정
- `ctrl option cmd U` 해당 함수만 테스트 실행

### iOS Runtime Header
- [iOS-Runtime-Headers](https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/UIKit.framework/UIViewController.h)

### 실습
- [실습](https://github.com/devxoul/FastLogin)


```
// 가장 인상 깊었던 코드 조각...🙏🏻
func testJoinButton_push() {
    //Given
    let navi = UIStoryboard(name: "Main", bundle: nil).instantiateInitialViewController() as! UINavigationController
    let viewController = navi.topViewController as! LoginViewController
    UIApplication.shared.keyWindow?.rootViewController = navi
    
    //When
    _ = viewController.view // load view
    print(viewController.value(forKey: "storyboardSegueTemplates"))
    let segueTemplates = viewController.value(forKey: "storyboardSegueTemplates") as? [AnyObject]
    let template = segueTemplates?.first as? AnyObject
    let targetActions = viewController.joinButton.value(forKey: "_targetActions") as? [AnyObject]
    let buttonTarget = targetActions?.first?.value(forKey: "_target") as? AnyObject
    print(targetActions)
    
    //XCTAssert(NSStringFromClass(type(of: template!))) ===
    XCTAssert(template === buttonTarget)
    viewController.joinButton.sendActions(for: .touchUpInside)
    
    //Wait
    let expectation = XCTestExpectation()
    DispatchQueue.main.asyncAfter(deadline: .now() + 0.5, execute: expectation.fulfill
    )
    XCTWaiter().wait(for: [expectation], timeout: 1)
    
    //Then
    XCTAssertEqual(navi.viewControllers.count, 2)
    XCTAssertEqual(navi.topViewController?.title, "Create Account")
}
```

```
func testWelcomeLabel_displayUsernameWhenFetchError() {
    //GIVEN
    let storyboard = UIStoryboard(name: "Profile", bundle: nil)
    let navi = storyboard.instantiateInitialViewController() as? UINavigationController
    let viewController = navi?.topViewController as! ProfileViewController
    
    let userService = StubUserService()
    userService.stubbedResult = .failure(NSError())
    viewController.userService = userService
    
    //WHEN
    _ = viewController.view // load view
    
    //THEN
    XCTAssertEqual(viewController.welcomeLabel.text, "Error!")
    //XCTAssertEqual(viewController.welcomeLabel.text?.lowercased(), "error")
    //XCTAssrt(viewController.priceLabel.text.contains("1,342")
}
```

<br>
<br>

## 강사님 프로필
- RxSwift [intmain](https://github.com/intmain)
- AutoLayout [KXCODING](https://kxcoding.com)
- TDD [devxoul](https://github.com/devxoul)