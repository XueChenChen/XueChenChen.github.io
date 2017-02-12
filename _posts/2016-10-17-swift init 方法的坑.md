---
layout:     post
title:      "swift中的init坑! "
date:       2016-10-17 20:00:00
author:     "Chen"
header-img: "img/skillTable/skillTable-bg.jpg"
tags:
    - 语言
    - swift
    - init
---

[Apple init 原网页](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html)

* 可失败构造器

  >init?()
  如果一个类、结构体或枚举类型的对象，在构造过程中有可能失败，则为其定义一个可失败构造器。这里所指的“失败”是指，如给构造器传入无效的参数值，或缺少某种所需的外部资源，又或是不满足某种必要的条件等。为了妥善处理这种构造过程中可能会失败的情况。你可以在一个类，结构体或是枚举类型的定义中，添加一个或多个可失败构造器。其语法为在init关键字后面添加问号(init?)。

  >init!()通常来说我们通过在init关键字后添加问号的方式（init?）来定义一个可失败构造器，但你也可以通过在init后面添加惊叹号的方式来定义一个可失败构造器（init!），该可失败构造器将会构建一个对应类型的隐式解包可选类型的对象。
你可以在init?中代理到init!，反之亦然。你也可以用init?重写init!，反之亦然。你还可以用init代理到init!，不过，一旦init!构造失败，则会触发一个断言。

* 必要构造器
  >init() 在类的构造器前添加required修饰符表明所有该类的子类都必须实现该构造器：


Swift 有超级严格的初始化方法。一方面，Swift 强化了 designated 初始化方法的地位。Swift 中不加修饰的 init 方法都需要在方法中保证所有非 Optional 的实例变量被赋值初始化，而在子类中也强制 (显式或者隐式地) 调用 super 版本的 designated 初始化，所以无论如何走何种路径，被初始化的对象总是可以完成完整的初始化的。

### 1.why 你重写init时required init?(coder aDecoder: NSCoder)必须写:

  ```objc
  required init?(coder aDecoder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
  }
  ```
  **Apple**
>Write the required modifier before the definition of a class initializer to indicate that every subclass of the class must implement that initializer.


*required 关键字强制子类对某个初始化方法进行重写*
`因为开发时我们常用UIKit框架,而UIKit框架UIView And Controller 都有init?(coder aDecoder: NSCoder)方法所以...`

>我们一开始虽然没有为指定构造器提供实现, 不过, 因为重载了指定构造器, 所以来自父类的指定构造器并不会被继承.如果子类没有定义任何的指定构造器, 那么会默认继承所有来自父类的指定构造器.
而 init(coder aDecoder: NSCoder) 方法是来自父类的指定构造器, 因为这个构造器是 required, 必须要实现. 但是因为我们已经重载了 init(), 定义了一个指定构造器, 所以这个方法不会被继承, 要手动覆写, 这就是第一个错误的原因.


### 2.在 Swift 中, 类的初始化有两种方式, 分别是

1. Designated Initializer
2. Convenience Initializer

Designated Initializer 指定构造器相当于Objective-C中的`NS_DESIGNATED_INITIALIZER`, 而 Convenience Initializer 译为便利构造器.

*指定构造器在一个类中必须至少有一个, 而便利构造器的数量没有限制.*
**Designated:**

>Designated initializers are the primary initializers for a class. A designated initializer fully initializes all properties introduced by that class and calls an appropriate superclass initializer to continue the initialization process up the superclass chain.

指定构造器是类的主要构造器, 要在指定构造器中初始化所有的属性, 并且要在调用父类合适的指定构造器.
example:
```objc
   {
     init()
     init(frame: CGRect)
     init?(coder aDecoder: NSCoder)
}
```

**convenience:**
>Convenience initializers are secondary, supporting initializers for a class. You can define a convenience initializer to call a designated initializer from the same class as the convenience initializer with some of the designated initializer’s parameters set to default values. You can also define a convenience initializer to create an instance of that class for a specific use case or input value type.

*所有的 convenience 初始化方法都必须调用同一个类中的 designated 初始化完成设置，另外 convenience 的初始化方法是不能被子类重写或者是从子类中以 super 的方式被调用的。*

```objc
class ClassA {
    let numA: Int
    init(num: Int) {
        numA = num
    }

    convenience init(bigNum: Bool) {
        self.init(num: bigNum ? 10000 : 1)
    }
}

class ClassB: ClassA {
    let numB: Int

    override init(num: Int) {
        numB = num + 1
        super.init(num: num)
    }
}
```
*只要在子类中实现重写了父类 convenience 方法所需要的 init 方法的话，我们在子类中就也可以使用父类的 convenience 初始化方法了。比如在上面的代码中，我们在 ClassB 里实现了 init(num: Int) 的重写。这样，即使在 ClassB 中没有 bigNum 版本的 convenience init(bigNum: Bool)，我们仍然还是可以用这个方法来完成子类初始化*

>其实不仅仅是对 designated 初始化方法，对于 convenience 的初始化方法，我们也可以加上 required 以确保子类对其进行实现。这在要求子类不直接使用父类中的 convenience 初始化方法时会非常有帮助。
### init 原则:

* **指定构造器必须调用它直接父类的指定构造器方法.**
* **便利构造器必须调用同一个类中定义的其它初始化方法.**
* **便利构造器在最后必须调用一个指定构造器.**


**我碰到此类问题时参考的文章**

[参考1:Swift 类构造器的使用](http://draveness.me/swift-zhong-init-de-shi-yong/)

[参考:2DESIGNATED，CONVENIENCE 和 REQUIRED](http://swifter.tips/init-keywords/)
