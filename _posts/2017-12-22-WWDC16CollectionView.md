---
layout: post
title: "Collection View"
author: "Amy"
---

> 16년도 WWDC의 UICollectionView 섹션을 다시 보고, CollectionView와 관련된 정보들을 아카이브 해야겠다고 생각해서 시작하는 포스팅입니다. 

#### [develpoer.apple.com :: Official Doc](https://developer.apple.com/documentation/uikit/uicollectionview)

# WWDC 2016
- [What's New in UICollectionView in iOS 10](https://developer.apple.com/videos/play/wwdc2016/219/)
- UICollectionView is a powerful class allowing your app to manage and customize the layout of views. iOS 10 brings enhancements for better performance, easier layout and brings features you've been looking for. Learn how to make your apps richer and faster by using new features in UICollectionView and its sibling, UITableView. *(WWDC 2016 - Session 219 - iOS, tvOS)*


## WWDC 2016 :: iOS10 CollectionView New Features
- `Smooth scrolling` : Scrolling like butter! 😂
- ⭐️ `UICollectionView Cell PreFetching`
- `Improvements to self-sizing cells`
- `Interactive reordering`
- `Embeded Refresh Control`


# Life Cycle of a Cell
- `prepareForReuse` ->
- `cellForItemAtIndexPath` ->
- `willDisplayCell` ->
- `didEndDisplayCell` (-> ⭐️ `willDisplayCell`)


# Self-Sizing Cells API
- ASIS :: layout.estimatedItemSize = CGSize(width:50,height:50)
- NEW :: layout.estimatedItemSize = `UICollectionViewFlowLayoutAutomaticSize`
- **Collection View will do the math for you**


# Interactive Reordering

- ```installsStandardGestureForInteractiveMovement = true```
-  moveItemAt to destinationIndexPath 이후 dataSource update 필요

```
override func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath) {
    dataSource.moveDataFrom(sourceIndexPath, toIndexPath: destinationIndexPath)
}
```

# Related Snippets 

## offset, inset, frame, bounds
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

## Pinning Header
- **`layout.sectionHeadersPinToVisibleBounds = true`**

## Editing Mode
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

## Custom FlowLayout

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

## Target ContentOffset

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


## Stretching Header

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
