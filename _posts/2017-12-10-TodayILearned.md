---
layout: post
title: "Today I Learned"
author: "younari"
---

> 2017.12.01 ~ 2017.12.07

#### 스크롤뷰 offset, inset, frame, bounds
- **contentInset**은 스크롤뷰의 콘텐츠 오프셋을 밀어내는 효과가 있다.
- **contentOffset**과 **scrollView bounds** 값은 함께 움직인다.
- **bounds**는 **subView**의 어느지점부터 스크린의 **CGRect**를 그리겠다는 선언과도 같다.

```
// print 찍어보면서 테스트 하기
scrollView.contentOffset.y: -64.0
scrollView.contentInset.top: 64.0
scrollView.bounds.origin.y: -64.0
scrollView.frame.origin.y: 0.0
```

#### 콜렉션뷰 헤더 앵커 잡기
- **`layout.sectionHeadersPinToVisibleBounds = true`**

```
override func viewDidLoad() {
	super.viewDidLoad()
	// Set up a 3-column Collection View
	let width = view.frame.size.width / 3
	let layout = collectionView?.collectionViewLayout as! UICollectionViewFlowLayout
	layout.itemSize = CGSize(width:width, height:width)
	layout.sectionHeadersPinToVisibleBounds = true
	// Refresh control
	let refresh = UIRefreshControl()
	refresh.addTarget(self, action: #selector(self.refresh), for: UIControlEvents.valueChanged)
	collectionView?.refreshControl = refresh
	// Toolbar
	navigationController?.isToolbarHidden = true
	// Edit
	navigationItem.leftBarButtonItem = editButtonItem
}
```



### 콜렉션뷰 Interactive Movement

- ```installsStandardGestureForInteractiveMovement = true```
-  단, movement 이후에 DataSource를 어떻게 처리할 지 잘 구현해주어야 한다.

```
override func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath)

```

### 콜렉션뷰 Delete & Insert

- `navigationItem.leftBarButtonItem = editButtonItem`

```
override func setEditing(_ editing: Bool, animated: Bool) {
		super.setEditing(editing, animated: animated)
		addButton.isEnabled = !editing
		collectionView?.allowsMultipleSelection = editing
		if !editing {
			navigationController?.isToolbarHidden = true
		}
		guard let indexes = collectionView?.indexPathsForVisibleItems else {
			return
		}
		for index in indexes {
			let cell = collectionView?.cellForItem(at: index) as! CollectionViewCell
			cell.isEditing = editing
		}
	}
```

### 콜렉션뷰 커스텀 플로우레이아웃

- `override func prepare()`
- ```override func layoutAttributesForElements(in rect: CGRect) -> [UICollectionViewLayoutAttributes]?```
- ```override open func shouldInvalidateLayout(forBoundsChange newBounds: CGRect) -> Bool```

```
override func prepare() {
    super.prepare() 
    if isSetup == false {
      setupCollectionView()
      isSetup = true
    }
  }
  
override func layoutAttributesForElements(in rect: CGRect) -> [UICollectionViewLayoutAttributes]? {

    let attributes = super.layoutAttributesForElements(in: rect)
    var attributesCopy = [UICollectionViewLayoutAttributes]()
    
    for itemAttributes in attributes! {
      let itemAttributesCopy = itemAttributes.copy() as! UICollectionViewLayoutAttributes
      changeLayoutAttributes(itemAttributesCopy)
      attributesCopy.append(itemAttributesCopy)
    }
    
    return attributesCopy
}
  
override open func shouldInvalidateLayout(forBoundsChange newBounds: CGRect) -> Bool {
	return true
}
```

### 콜렉션뷰 Target ContentOffset

- ```override func targetContentOffset(forProposedContentOffset proposedContentOffset: CGPoint, withScrollingVelocity velocity: CGPoint) -> CGPoint```

```
override func targetContentOffset(forProposedContentOffset proposedContentOffset: CGPoint, withScrollingVelocity velocity: CGPoint) -> CGPoint {
    
    let layoutAttributes = self.layoutAttributesForElements(in: collectionView!.bounds)
 
    let center = collectionView!.bounds.size.height / 2
   
     // proposedContentOffset.y 일 때의 Center
    let proposedContentOffsetCenterOrigin = proposedContentOffset.y + center
    
    // 가장 가까운 item과의 거리
    let closest = layoutAttributes!.sorted { abs($0.center.y - proposedContentOffsetCenterOrigin) < abs($1.center.y - proposedContentOffsetCenterOrigin) }.first ?? UICollectionViewLayoutAttributes()
  
    let targetContentOffset = CGPoint(x: proposedContentOffset.x, y: floor(closest.center.y - center))
    return targetContentOffset
}
```


### 콜렉션뷰 Stretching Header

- `let offset = collectionView!.contentOffset`
- `deltaY = fabs(offset.y)`

```
override func layoutAttributesForElements(in rect: CGRect) -> [UICollectionViewLayoutAttributes]? {
    let layoutAttributes = super.layoutAttributesForElements(in: rect)! as [UICollectionViewLayoutAttributes]
    
    let offset = collectionView!.contentOffset
    if (offset.y < 0) {
      let deltaY = fabs(offset.y)
      for attributes in layoutAttributes {
        if let elementKind = attributes.representedElementKind {
          if elementKind == UICollectionElementKindSectionHeader {
            var frame = attributes.frame
            frame.size.height = max(0, headerReferenceSize.height + deltaY)
            print("frame.origin.y: \(frame.origin.y)")
            print("frame.minY: \(frame.minY)")
            print("deltaY: \(deltaY)")
            print("frame.minY - deltaY: \(frame.minY - deltaY)")
            frame.origin.y = frame.minY - deltaY
            print("frame.origin.y: \(frame.origin.y)")
            attributes.frame = frame
          }
        }
      }
    }
    
    return layoutAttributes
}
```
