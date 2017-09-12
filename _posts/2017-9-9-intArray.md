---
layout: post
title: 배열 속 Int 다루기
---

> 배열에 담긴 각각의 숫자들의 대소를 전후 비교하거나, 연속된 중복 숫자를 없애서 반환하는 등의 함수입니다.

## 배열에 담긴 숫자들의 대소 비교

```swift
func compareNext(inputArray: [String]) {
    var resultArray: [String] = []
    let count: Int = inputArray.count
    for i in 0..<count {
    if i==0 {
		resultArray.append(inputArray[0])
	}else if Int(inputArray[i])! < Int(inputArray[i-1])! {
        resultArray.append(inputArray[i])
	}
    }
	print(resultArray)
}
compareNext(inputArray: ["1", "3", "2", "8", "9"])
```

## 연속으로 중복된 숫자 없애기
- 입력받은 숫자를 숫자의 배열로 만들어주는 함수

```swift
func intToArr(num: Int) -> Array<Int> {
    
    var intArr: [Int] = []
    var number: Int = num
    while number > 0 {
        intArr.insert(number%10, at: 0)
        number = number/10
    }
    return intArr
}
```
- 제곱수를 만들어주는 함수 (num1의 num2승을 만들어주는 함수)

```swift
func square(value: Int, exponent: Int) -> Int {
    
    var result: Int = 1
    for _ in 0..<exponent {
        result *= value
    }
    
    return result
}
```

- 연속으로 중복된 숫자를 없애서 Int로 최종 반환해주는 함수

```swift
func shorter(num: Int) -> Int {
    
    let number: Int = num
    var originalArr: [Int] = intToArr(num: number)
    var shortArr: [Int] = []
    
    // 중복 제거한 숫자들을 shortArr에 넣는다.
    for i in 0..<originalArr.count {
        if i == 0 {
            shortArr.insert(originalArr[i], at: 0)
        }else if originalArr[i-1] != originalArr[i] {
            shortArr.insert(originalArr[i], at: 0)
        }
    }
    // 배열에 담긴 숫자들을 Int로 변환하여 최종 출력
    var returnValue: Int = 0
    for j in 0..<shortArr.count {
        returnValue += shortArr[j] * square(value: 10, exponent: j)
    }
    return returnValue
}
```

## 배열의 최대값과 최소값을 반환하기

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```
