# AboutSpriteKit

- 建立一个SpriteKit项目时，推荐文件结构如下
```
-AllPoj
  -Sprites(用于存放精灵类)
  -Scenes(用于存放显示的场景)
  -ViewControllers(用于存放控制器的配置和场景的加载)
  -Suppor(用于存放各种支持文件)
```

- 在控制器上加载一个场景常见的方法
```Swift
        // 设置屏幕显示的大小(GameScene是一个场景的类)
        let sceneNode = GameScene(size: view.frame.size)

        if let view = self.view as! SKView? {
            view.presentScene(sceneNode)
            // 设置为true时，同一个Z深度的子节点的顺序不会被保证
            view.ignoresSiblingOrder = true
            // 显示物理实体,多用于测试
            view.showsPhysics = true
            // 显示帧率
            view.showsFPS = true
            // 显示节点(精灵)的数量
            view.showsNodeCount = true
        }
```

- 控制器中三个常常使用的重载方法
```Swift
    /// 是否允许自动旋转
    override var shouldAutorotate: Bool {
        return true
    }

    /// 设置屏幕朝向
    override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return .landscape//横屏
    }

    /// 是否隐藏状态条
    override var prefersStatusBarHidden: Bool {
        return true
    }
```

- 场景类中三个常常使用的重载方法
```Swift
    /// 手指按下时
    ///
    /// - parameter touches:
    /// - parameter event:
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    }
    
    /// 手指移动时
    ///
    /// - parameter touches:
    /// - parameter event:
    override func touchesMoved(_ touches: Set<UITouch>, with event: UIEvent?) {
    }
    
    /// 屏幕更新时
    ///
    /// - parameter currentTime:
    override func update(_ currentTime: TimeInterval) {
    }

```

- 场景类中的碰撞检测
```
//1. 先设置需要检测碰撞的物体的类别以及与其碰撞的类别
let bodyOne:UInt32 = 0x1<<1 //1
let bodyTwo:UInt32 = 0x1<<2 //2
xxx1.physicsBody.categoryBitMask = bodyOne
xxx1.physicsBody.contactTestBitMask = bodyTwo
xxx2.physicsBody.categoryBitMask = bodyTwo
xxx2.physicsBody.categoryBitMask = bodyOne
//2. 取得当前物理世界(场景)的代理
self.physicsWorld.contactDelegate = self
//3. 在类中加入碰撞协议
class xxx: SKPhysicsContactDelegate
//4. 实现碰撞协议函数
func didBegin(_ contact:SKPhysicsContact) {
}
```

- 将一个精灵移除碰撞
```Swift
//1. 通用写法
xxx.physicsBody.collisionBitMask = 0
xxx.physicsBody.categoryBitMask = 0
//2. 碰撞检测中的写法
contact.bodyA.node.physicsBody.collisionBitMask = 0
contact.bodyA.node.physicsBody.categoryBitMask = 0
```

- 将一个精灵移除内存
```Swift
//1. 通用写法
xxx.removeFromParent()
xxx.physicsBody = nil
xxx.removeAllActions()
//2. 碰撞检测中的移除
contact.bodyA.node.removeFromParent()
contact.bodyA.node.physicsBody = nil
contact.bodyA.node.removeAllActions()
```
