---
layout: post
title: "Caesar cipher 알고리즘"
author: "younari"
---

> 시저 암호: 어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 함수입니다. 대소문자 또한 고려했습니다.

## 입력값과 알파벳을 비교하여, Key 만큼 밀린 배열을 반환하는 함수

{% highlight swift %}
func ceasar(data:[String], keyNum:Int) -> Array<String>
{
    let alphabet:[String] = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"]
    let Alphabet:[String] = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"]

    // String을 담은 배열을 하나 만들어서, 알파벳 배열과 비교
    var dataCeasar:[String] = data
    var dataResult:[String] = []

    for i in 0..<dataCeasar.count {
        for j in 0..<26 {
            
            if (j < 26-keyNum) && (alphabet[j] == dataCeasar[i]) {
                dataResult.append(alphabet[j+keyNum])
            }else if (j < 26-keyNum) && (Alphabet[j] == dataCeasar[i]) {
                dataResult.append(Alphabet[j+keyNum])
            }else if (j >= 26-keyNum) && (alphabet[j] == dataCeasar[i]) {
                dataResult.append(alphabet[(j+keyNum)%26])
            }else if (j >= 26-keyNum) && (Alphabet[j] == dataCeasar[i]) {
                dataResult.append(Alphabet[(j+keyNum)%26])
            }
        }
    }
    
    return dataResult
}

print(ceasar(data:["a", "B", "W", "z"], keyNum:4))
{% endhighlight %}


## 알파벳 배열 쉽게 만들기 
{% highlight swift %}
var alphabet: [Character] = []

for abc in "abcdefghijklmnopqrstuvwxyz".characters
{
    alphabet.append(abc)
}
print(alphabet)
{% endhighlight %}