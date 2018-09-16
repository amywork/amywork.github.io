---
layout: post
title: "Cell Delegate 실습"
author: "Amy"
---

# CustomCell설계 및 delegate 패턴 실습

### CustomCell의 스위치 value가 바뀔 때마다 ViewController에게 value 값을 전달해주기 위해, delegate 패턴을 만들어본다.

# 01. UITableViewCell을 상속받은 CustomCell설계

### CustomCellDelegate 프로토콜
{% highlight swift %}
protocol CustomCellDelegate {
    func customCellSwitched(_ cell:CustomCell, didChangedSwitch value:Bool)
}
{% endhighlight %}

### CustomCell에서 delegate 속성 설정
{% highlight swift %}
import UIKit
class CustomCell: UITableViewCell {
    
    @IBOutlet var switcher: UISwitch!
    
    var cellDelegate:CustomCellDelegate?
    var indexPath:IndexPath?
    var switchData:Bool = true {
        willSet {
            self.switcher.isOn = newValue
        }
    }
    
    override func awakeFromNib() {
        super.awakeFromNib()
    }

    @IBAction func switchBtnHandler(_ sender: UISwitch) {
        cellDelegate?.customCellSwitched(self, didChangedSwitch: sender.isOn)
    }
}
{% endhighlight %}


# 02. ViewController에서 CustomCellDelegate 프로토콜의 메소드 구현
- CustomCellDelegate 프로토콜 채택 및, 프로토콜이 강제하는 메소드 customCellSwitched 구현

{% highlight swift %}
import UIKit
class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource, CustomCellDelegate {
    
    var onOffList:[Bool] = [true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true] 
    // 데이터는 테스트용으로 스위치의 Bool값 설정

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return onOffList.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "CustomCell", for: indexPath) as! CustomCell
        cell.cellDelegate = self
        cell.switchData = onOffList[indexPath.row]
        cell.textLabel?.text = "\(indexPath.row)" + "Welcome to Delegate World!"
        cell.indexPath = indexPath
        return cell
    }
    
    
    func customCellSwitched(_ cell: CustomCell, didChangedSwitch value: Bool)
    {
        onOffList[cell.indexPath!.row] = value
    }
    
}
{% endhighlight %}