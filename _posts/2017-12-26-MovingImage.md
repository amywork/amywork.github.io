---
layout: post
title: "Moving Image"
author: "Amy"
---

> SNS 피드처럼, 이미지뷰의 사진을 클릭하면 해당 콘트롤러 정중앙으로 사진이 확대되고, 다시 클릭하면 제자리로 돌아가는 UI를 구현해본다.


# View Property
- `cellImageView` : 실제 이미지뷰
- `zoomImageView` : 콘트롤러 중앙에서 Zooming될 이미지뷰
- `blackBackgroundView` : `zoomImageView` 위아래로 깔리는 BG 이미지

```
let blackBackgroundView = UIView()
let zoomImageView = UIImageView()
    
let cellImageView: UIImageView =  {
    let view = UIImageView()
    view.image = UIImage(named: "shortcut")
    view.clipsToBounds = true
    view.contentMode = .scaleAspectFill
    return view
}()
```

# `viewDidLoad()`
- `cellImageView` 의 오토 레이아웃 잡고,
- `isUserInteractionEnabled` true로 해준 뒤, 
- `tapGesture` 추가

```
override func viewDidLoad() {
    super.viewDidLoad()
    view.addSubview(cellImageView)
    cellImageView.isUserInteractionEnabled = true
    cellImageView.translatesAutoresizingMaskIntoConstraints = false
    cellImageView.widthAnchor.constraint(equalTo: view.widthAnchor, multiplier: 0.5).isActive = true
    cellImageView.heightAnchor.constraint(equalToConstant: 100).isActive = true
    cellImageView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor).isActive = true
    cellImageView.leftAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leftAnchor).isActive = true
    let tapGesture = UITapGestureRecognizer(target: self, action: #selector(animate))
    cellImageView.addGestureRecognizer(tapGesture)
}
```

# `animate()`

- `cellImageView`의 tap gesture에 추가할 selector method

```
@objc func animate() {
    
    cellImageView.alpha = 0
    
    blackBackgroundView.frame = self.view.frame
    blackBackgroundView.backgroundColor = .black
    blackBackgroundView.alpha = 0
    view.addSubview(blackBackgroundView)
    
    zoomImageView.image = UIImage(named: "shortcut")
    zoomImageView.clipsToBounds = true
    zoomImageView.contentMode = .scaleAspectFill
    zoomImageView.frame = cellImageView.frame
    view.addSubview(zoomImageView)
    
    zoomImageView.isUserInteractionEnabled = true
    zoomImageView.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(zoomOut)))
    
    UIView.animate(withDuration: 1,
                   delay: 0,
                   usingSpringWithDamping: 10,
                   initialSpringVelocity: 1,
                   options: .curveEaseIn, animations: {
                    
                    let height = self.view.frame.size.width / self.cellImageView.frame.size.width * self.cellImageView.frame.size.height
                    
                    self.zoomImageView.frame = CGRect(x: 0, y: (self.view.frame.height/2)-(height/2), width: self.view.frame.size.width, height: height)
                    
                    self.blackBackgroundView.alpha = 0.6
                    
    }, completion: nil)

}
```

# `zoomOut()`

- `zoomImageView`의 tap gesture에 추가할 selector method


```
@objc func zoomOut() {
  
    UIView.animate(withDuration: 1,
                   delay: 0,
                   usingSpringWithDamping: 10,
                   initialSpringVelocity: 1,
                   options: .curveEaseIn, animations: {
                    self.zoomImageView.frame = self.cellImageView.frame
                    self.blackBackgroundView.alpha = 0
    }, completion:  { (didComplete) -> Void in
        self.zoomImageView.removeFromSuperview()
        self.blackBackgroundView.removeFromSuperview()
        self.cellImageView.alpha = 1
    })
    
}
```