---
layout: post
title: "스위프트 Table View"
author: "Amy"
---

# UITableView

- [UITableView Xcode 프로젝트 바로가기](https://github.com/amywork/tastySwift/tree/master/0929_TableView/0929_TableView)

## override func viewDidLoad()
- 채택해야 할 프로토콜: UITableViewDataSource, UITableViewDelegate
- TableView의 frame과 스타일을 init 하며 인스턴스 생성
- `let tableView: UITableView = UITableView(frame: view.bounds, style: .plain)`
- **delegate**: How the table is displayed
- **dataSource**: The data that is displayed inside the cells.
- UITableViewDataSource 선언 `tableView.dataSource = self` 
- UITableViewDelegate 선언 `tableView.delegate = self`
- `dequeueReusableCell` 에서 UI를 재사용 할 수 있도록 cell 정보와 관련된 class를 register (등록)
- `tableView.register(MenuCell.self, forCellReuseIdentifier: "MenuCell")`
- `tableView.register(UITableViewCell.self, forCellReuseIdentifier: "UITableViewCell")`
- 위 코드에서 ReuseIdentifier의 이름은 통상 클래스 이름과 동일하게 부여한다.
- 위 코드에서 UITableViewCell은 시스템 cell 클래스를 뜻한다.
- MenuCell은 테스트를 위해 샘플로 만든 커스텀 클래스이다.
- -> UITableViewCell.self 에서 self는 클래스 자기 자신. 즉 class이름.self = 클래스 자체
- `view.addSubview(tableView)`

{% highlight swift %}
override func viewDidLoad() {
    super.viewDidLoad()
    let tableView: UITableView = UITableView(frame: view.bounds, style: .plain)
    tableView.dataSource = self // UITableViewDataSource 선언
    tableView.delegate = self // UITableViewDelegate 선언
    tableView.register(menuCell.self, forCellReuseIdentifier: "menuCell")
    tableView.register(UITableViewCell.self, forCellReuseIdentifier: "UITableViewCell") // tableView에 Cell Class를 Register
    view.addSubview(tableView)
}
{% endhighlight %}



## 01. numberOfRowsInSection
- 섹션의 갯수(option), 섹션의 Row 갯수(required)를 설정한다.

{% highlight swift %}
let menu: [String] = ["1", "2", "3", "4", "5"]

func numberOfSections(in tableView: UITableView) -> Int {
    return 2
}

func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return menu.count
}
{% endhighlight %}


## 02. cellForRowAt indexPath
- row의 cell을 만든다. (return UITableViewCell)
- tableView.dequeueReusableCell: que 자료구조에서 재사용 cell 데이터를 끄집어내는 행위

{% highlight swift %}
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    if indexPath.section == 0 {
        let cell = tableView.dequeueReusableCell(withIdentifier: "UITableViewCell", for: indexPath)
        cell.textLabel?.text = "〰️👋🏻 \(menu[indexPath.row])"
        return cell
    }else {
        let cell = tableView.dequeueReusableCell(withIdentifier: "menuCell", for: indexPath) as! menuCell
        cell.setImageName(name: menu[indexPath.row])
        return cell
    }
}    
{% endhighlight %}


## 03. heightForRowAt indexPath
- Row의 높잇값을 결정한다.

{% highlight swift %}
func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
    // let cell: menuCell = tableView.cellForRow(at: indexPath) as! menuCell
    return 250
}
{% endhighlight %}


## 04. didSelectRowAt indexPath
- Cell을 선택했을 시의 행위를 정의한다.

{% highlight swift %}
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    let name: String = menu[indexPath.row]
    let alert = UIAlertController(title: "확인🙃👀", message: "\(name)가 선택되었습니다.", preferredStyle: .alert)
    let okAction = UIAlertAction(title: "OK", style: .default, handler: nil)
    alert.addAction(okAction)
    present(alert, animated: true, completion: nil)
}
{% endhighlight %}


## 05. titleForHeaderInSection
- Header 이름을 정의할 수 있다. 

{% highlight swift %}
func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
    <#code#>
}
{% endhighlight %}


## 06. canEditRowAt
- Cell에 밀어서 삭제 기능을 넣을 수 있다.

{% highlight swift %}
func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {
    <#code#>
}
{% endhighlight %}


## 07. scrollViewDidScroll
- scroll이 일정 offset에 다다르면, 데이터를 추가로 요청할 수 있다.

{% highlight swift %}
func scrollViewDidScroll(_ scrollView: UIScrollView) {
    if contentSize.offset.y < 100 {
        data 추가 요청
    }
}
{% endhighlight %}

