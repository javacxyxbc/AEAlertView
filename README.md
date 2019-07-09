# AEAlertView
Custom AlertView supports multiple modes 自定义AlertView 支持多种模式弹窗 
If your swift version is less than 4, you can  
 如果你的swift版本是4.0以下, 你可以 pod'AEAlertView','1.0' 
如果是 swift 4.0 以上 5.0 以下  你可以 pod'AEAlertView','1.7' 
如果是 swift 5.0 使用最新版即可

- **在最新版本中 新增了 仿UI的弹窗,  开放了AEBaseAlertView 如果你的需求 AEAlertView和AEUIAlertView中没有 你可以继承AEBaseAlertView 来实现属于你自己的AlertView **

先看新版的图片 
<view>
<img src="https://github.com/Allen0828/AEAlertView/blob/master/img-folder/alertView_GIF.gif" width="200" >  </img>  <img src="https://github.com/Allen0828/AEAlertView/blob/master/img-folder/uiAlertView_GIF.gif" width="200">  </img>
</view>

演示的样式 在Demo中都有  所有的属性都有说明 希望大家可以支持一下. 
如果有 你有任何问题 或者 好的建议 欢迎联系我  - email: allen.zhang0828@gmail.com -

-------------------
*** 控件支持 设置左右 上下间距  本来想提供 类方法来一句话调用, 但是每个项目的主题色不同 所有需要你 自己设置自己的主题色***



----以下是1.0版本时的说明-----
代码调用方式 是仿系统的 如果你遇到的需求是 全局使用同样样式的弹窗 你可以封装一个Manger 来管理  代码底层使用VFL 你可以自己修改他的尺寸,

Code calls are system-like if you encounter a requirement that the same style of pop-up windows be used globally you can encapsulate a Manger to manage the underlying code using VFL and you can modify its size yourself.

当然你也可以创建一个管理类来保存全局的统一性 并且提供快速调用类方法
``` swift
import UIKit
import AEAlertView

class AlertView: AEAlertView {
    
    ///MARK: 快速调用
    public class func alert(_ title: String, _ message: String, _ actions:[String], handler:((AEAlertAction)->Void)?) {
        
        let alertView = AlertView()
        alertView.title = title
        alertView.message = message
        
        let cancel = AEAlertAction(title: actions[0], style: .Cancel) { (action) in
            print("取消")
            alertView.close()
        }
        alertView.addAction(action: cancel)
        
        if actions.count > 1 {
            let def = AEAlertAction(title: actions[1], style: .Default) { (action) in
                if handler != nil {
                    handler!(action)
                }
                alertView.close()
            }
            alertView.addAction(action: def)
        }
        
        alertView.show()
    }

    
    override init(alertViewStyle: AEAlertViewStyle, title: String?, message: String?) {
        super.init(alertViewStyle: alertViewStyle, title: title, message: message)
        
        self.buttonColor = UIColor.red
        self.cancelButtonColor = UIColor.white
        self.cancelButtonLayerBorderColor = UIColor.red
        self.cancelButtonTitleColor = UIColor.red
        self.titleTopMargin = 16
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
}
```

在外部调用示例
``` swift
AlertView.alert("提示", "是否去分享", ["取消","确认"]) { (action) in
            print("确认")
        }
```

复制代码看看效果吧


如果是局部调用的话 只要简单实现就可以

``` swift
let view = AEAlertView(alertViewStyle: .Default)

view.title = "Title"
view.message = "Message"

let cancel = AEAlertAction(title: "cancel", style: .Cancel) { (action) in
  view.close()
  print("cancel 点击") 
}
view.addAction(action: cancel)
            
let confirm = AEAlertAction(title: "confirm", style: .Default) { (action) in
  print("confirm 点击")
}
view.addAction(action: confirm)

view.show()
```


``` swift
let view = AEAlertView()
view.title = "自定义"
view.message = "自定义距离 颜色 样式 Custom distance color style"
view.titleFont = UIFont.systemFont(ofSize: 20)
view.titleColor = UIColor.blue
view.messageFont = UIFont.systemFont(ofSize: 18)
view.messageColor = UIColor.orange

view.titleTopMargin = 6
view.messageWithButtonMargin = 6

view.cancelButtonColor = UIColor.orange
view.cancelButtonTitleColor = UIColor.white
view.buttonColor = UIColor.red
view.buttonTitleColor = UIColor.white
view.buttonLayerBorderColor = nil     //设置为nil 取消描边


let cancel = AEAlertAction(title: "cancel", style: .Cancel) { (action) in
    view.close()
    print("cancel 点击")
}
view.addAction(action: cancel)

let confirm = AEAlertAction(title: "confirm", style: .Default) { (action) in

    print("confirm 点击")
}
view.addAction(action: confirm)

view.show()
```




``` swift
let view = AEAlertView()
view.title = "Too much content"
view.message = "内容太多时 设置messageHeight 可以让内容超出部分滚动 When messageHeight is too much content, the content can be rolled beyond the part."
view.messageHeight = 100

let cancel = AEAlertAction(title: "cancel", style: .Cancel) { (action) in
    view.close()
    print("cancel 点击")
}
view.addAction(action: cancel)

let confirm = AEAlertAction(title: "confirm", style: .Default) { (action) in

    print("confirm 点击")
}
view.addAction(action: confirm)

view.show()
```

