# Event事件

QEvent类型

事件不用绑定信号和槽，直接重写事件所对应的函数即可

小技巧：使用类似%d的功能定义带参数的字符串(格式化字符串)

- `QString str = QString(x=%1,y=%2).arg(x).arg(y);`

- 不用管参数是什么类型，后面第一个.arg()的值给%1，第二个给%2......

## Qt中的鼠标事件

想要捕捉一个控件的鼠标事件，那个控件首先必须是自定义控件。自定义控件不想用ui也可以直接创建C++ class 然后提升为。

- 鼠标进入：enterEvent

    `[virtual protected] void enterEvent(QEvent *event);`

    在QWidget类中，虚函数说明可以重写该函数，自定义鼠标进入后要做的事情

- 鼠标离开：leaveEvent

    `void myLable::leaveEvent(QEvent *event){要实现的功能};`

- 鼠标按下：mousePressEvent

    `void mousePressEvent(QMouseEvent *ev)`

    ev里面储存了很多信息
    - 鼠标按下的x坐标：`ev.x()`
    - 鼠标按下的y坐标：`ev.y()`
    - 基于整个屏幕的坐标：`ev.globalX()`/`ev.globalY()`
    - 判断左右键：`ev.button()`

        返回值类型为MouseButton，里面有很多枚举值
        - Qt:LeftButton：左键
        - Qt:RightButton:右键
        - Qt:MidButton：鼠标的中间滚轮

        可以这样判断:`if(Qt::LeftButton == ev.button){}`

- 鼠标释放：mouseReleaseEvent

- 鼠标移动：mouseMoveEvent

    按住鼠标进行移动，可以在构造函数中设置鼠标追踪状态`setMouseTracking(true);`，实现不按下鼠标也能追踪鼠标的移动

    移动是一个状态，不能用与按下相同的方式来判断是否是左键，而应该用:
    
    `if(ev->buttons() & Qt::LeftButton)`，
    
    其中&是位与运算符
***


## Event事件分发器

在程序接收到事件 -> 事件被触发之间有一个事件分发的函数

`bool event(QEvent* ev)`

如果返回的是true，代表用户要自己处理这个事件，不向下分发事件。这意味着我们可以在这里进行拦截

拦截操作：

```C++
bool myLable::event(QEvent *ev){

    // 拦截鼠标按下事件
    if(ev->type() == QEvent::MouseButtonPress){
        ......

        return true;
    }

    // 其它事件不进行拦截，交给父类默认处理
    return QLable::event(ev);
}
```

一般不拦截，直接重写事件函数就行了

## 事件过滤器

可以在程序分发到event事件分发器之前再做一次高级的拦截 

使用步骤：
1. 给控件安装事件过滤器

    `ui->lable->installEventFilter(this);`

    参数是谁给这个控件安装事件过滤器

2. 重写eventFilter事件

    `bool eventFilter(QObject*, QEvent *);`

    也是return true不往下分发

    ```C++
    bool Widget::eventFilter(QObject* obj, QEvent * e){
        if(obj == ui->lable){
            if(e->type() == QEvent::MouseButtonPress){
                ......
                return true;
            }
        }

        return QWidget::eventFilter(obj,e);
    }
    ```
***
***
