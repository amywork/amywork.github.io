---
layout: post
title: "은행 계좌 클래스 만들기"
author: "younari"
---


### 이름, 금액을 가진 "계좌" 클래스 만들기

{% highlight swift %}
class BankAccount {
    var bankName: String = ""
    var deposit: Double = 0
    
    init(name: String, deposit: Double) {
        self.bankName = name
        self.deposit = deposit
    }
}
{% endhighlight %}

### 이름, 주소, 복수개 계좌의 배열, 총금액을 가진 "고객" 클래스 만들기

{% highlight swift %}

class Customer {
    var name: String = ""
    var address: String = ""
    var accounts: [BankAccount] = []
    var totalCash: Double = 0
    
    init(name: String) {
        self.name = name
    }
    
    
    // method 01. 보유 계좌 한방에 추가하기
    // 계좌 설정과 동시에 총금액에도 금액 넣어주기
    func setAccounts(accounts: [BankAccount]) {
        self.accounts = accounts
        getTotalCash()
    }
    
    // method 02. 보유 계좌 1개씩 추가
    // 계좌 설정과 동시에 총금액에도 금액 넣어주기    
    func setAccount(account: BankAccount) {
        self.accounts.append(account)
        self.totalCash += account.deposit
    }
    
    // method 03. 보유 계좌들의 금액을 모두 더한, totalCash를 구해주는 함수
    // 계좌 설정과 동시에 실현시킬 함수입니다.    
    func getTotalCash() {
        for account in accounts {
        totalCash += account.deposit
        }
    }
}

{% endhighlight %}

### 인스턴스 생성하여 고객별 보유 금액 구하기

{% highlight swift %}

var person1: Customer = Customer(name: "Steve")
var person2: Customer = Customer(name: "Tim")

var account1: BankAccount = BankAccount(name: "Shinhan", deposit: 120000)
var account2: BankAccount = BankAccount(name: "Hana", deposit: 120000)
var account3: BankAccount = BankAccount(name: "Kakao Bank", deposit: 120000)

var account4: BankAccount = BankAccount(name: "K Bank", deposit: 120000)
var account5: BankAccount = BankAccount(name: "Woori", deposit: 120000)

var account6: BankAccount = BankAccount(name: "Jeju", deposit: 120000)

person1.setAccounts(accounts: [account1, account2, account3])
person2.setAccounts(accounts: [account4, account5])
person1.setAccount(account: account6)

print(person1.totalCash)
print(person2.totalCash)

{% endhighlight %}

### 돈을 이체하는 함수를 가진 클래스 만들기
- 단, 이체 후 고객의 totalCash를 refresh 해주어야 한다.

{% highlight swift %}

class Calculation {
          
            func transferMoney(toAccount: BankAccount, fromAccount: BankAccount, amounts: Double) {
                fromAccount.deposit -= amounts
                toAccount.deposit += amounts
            }
        }

{% endhighlight %}
