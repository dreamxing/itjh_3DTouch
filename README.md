#### 昨天闲来无事，对着自己的iPhone6s,准备给IT江湖加入3D Touch吧！！

 * icon 重按弹出菜单
 * tableView Cell 轻按按预览文章详情
 * tableView Cell 轻按按预览文章详情,上滑底部显示Action
 * tableView Cell 轻按按预览文章详情,重按进入文章详情页
 
## icon 重按弹出菜单
 
代码片段:

在`AppleDelegate`文件中

```swift

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
        // Override point for customization after application launch.
        
        //添加icon 3d Touch
        let firstItemIcon:UIApplicationShortcutIcon = UIApplicationShortcutIcon(type: .Share)
        let firstItem = UIMutableApplicationShortcutItem(type: "1", localizedTitle: "分享", localizedSubtitle: nil, icon: firstItemIcon, userInfo: nil)
        
        let firstItemIcon1:UIApplicationShortcutIcon = UIApplicationShortcutIcon(type: .Compose)
        let firstItem1 = UIMutableApplicationShortcutItem(type: "2", localizedTitle: "编辑", localizedSubtitle: nil, icon: firstItemIcon1, userInfo: nil)
        
        
        application.shortcutItems = [firstItem,firstItem1]
        
        return true
    }
    
    /**
    3D Touch 跳转
    
    - parameter application:       application
    - parameter shortcutItem:      item
    - parameter completionHandler: handler
    */
    func application(application: UIApplication, performActionForShortcutItem shortcutItem: UIApplicationShortcutItem, completionHandler: (Bool) -> Void) {
        
        let handledShortCutItem = handleShortCutItem(shortcutItem)
        completionHandler(handledShortCutItem)
        
    }
    
    func handleShortCutItem(shortcutItem: UIApplicationShortcutItem) -> Bool {
        var handled = false

        if shortcutItem.type == "1" { //分享

            let rootNavigationViewController = window!.rootViewController as? UINavigationController
            let rootViewController = rootNavigationViewController?.viewControllers.first as UIViewController?

            rootNavigationViewController?.popToRootViewControllerAnimated(false)
            rootViewController?.performSegueWithIdentifier("toShare", sender: nil)
            handled = true
            
        }
        
        if shortcutItem.type == "2" { //编辑

            let rootNavigationViewController = window!.rootViewController as? UINavigationController
            let rootViewController = rootNavigationViewController?.viewControllers.first as UIViewController?

            rootNavigationViewController?.popToRootViewControllerAnimated(false)
            rootViewController?.performSegueWithIdentifier("toCompose", sender: nil)
            handled = true
            
        }
        return handled
    }
    

 
## tableView Cell 轻按按预览文章详情

代码片段:

```swift

    func previewingContext(previewingContext: UIViewControllerPreviewing, viewControllerForLocation location: CGPoint) -> UIViewController? {
   
        
        let cellPosition = tableView.convertPoint(location, fromView: view)
        
        if let touchedIndexPath = tableView.indexPathForRowAtPoint(cellPosition) {
            
            tableView.deselectRowAtIndexPath(touchedIndexPath, animated: true)
            
            let aStoryboard = UIStoryboard(name: "Main", bundle:NSBundle.mainBundle())
            
            if let myVC = aStoryboard.instantiateViewControllerWithIdentifier("ArtilceShowView") as? ArticleShowViewController  {
                
                myVC.urlStr = "https://www.baidu.com"
                
                let cellFrame = tableView.cellForRowAtIndexPath(touchedIndexPath)!.frame
                previewingContext.sourceRect = view.convertRect(cellFrame, fromView: tableView)
                
                
                
                return myVC  
            }  
        }
        
        return UIViewController()
    }




