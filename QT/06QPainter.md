# QPainter 绘图   
## 基本绘图事件
1. 在widget中重写绘图事件函数,系统会自动调用，不重写就调用空实现

    `void Widget::paintEvent(QPainEvent);`

    如果想在构造函数里重新调用，使用`this->update();`

2. 包含画家类头文件

    `#include<QPainter>`

3. 实例化画家对象

    `QPainter painter(this);`

    this指定的是在哪里绘图，在this中的空窗口中画画

4. 画一条线

    `painter.drawLine(QPoint(x,y),QPoint(x,y))`

5. 画一个椭圆(圆)

    `painter.drawEilipse(QPoint(圆心坐标),长轴,短轴);`

6. 矩形

    `painter.drawRect(QRect(左上点,右下点));`

    有很多重载的画矩形方式

7. 画文字

    `painter.drawText();`
    - 参数一：QRect();画一个矩形
    - 参数二：QString，矩形框里写的字

8. 设置画笔

    `QPen pen(QColor(255,0,0));`

    - QPen类中还有许多可以设置的选项

    让画家拿这根笔画图

    `painter.setPen(pen);`

9. 利用画刷填充颜色

    `QBrush brush();`

    让画家使用画刷

    `painter.setBrush(brush);`

    此时画出来的封闭图形都是填充完毕的实心的

## 绘图高级设置

- 抗锯齿

    `painter.setRenderKint();`

    参数是一些枚举值

    会导致效率较低

- 让画家移动起笔位置

    `painter.translate(x,y);`

- 保存和还原画家状态

    保存画家的当前状态：`painter.save()`

    还原保存的画家状态：`painter.restore()`

## 手动调用绘图事件(画资源图片)

- 绘制资源图片
    `painter.drawPixmap(x,y,QPixmap("资源路径"));`

- 重新调用绘图事件

    `void QWidget::repaint()`

## 用画家类绘制窗口背景图片模板：
```C++
#include<QPainter>

void MainWindow::paintEvent(QPaintEvent *){
    QPainter *painter = new QPainter(this);
    // 画背景
    QPixmap background;
    if(!background.load(":/res/MainSceneBackgroundImg.jpg")){
        qDebug()<<"MainSceneBackgroundImg加载失败";
    }
    painter->drawPixmap(0,0,this->width(),this->height(),background); 
    // 前两个参数是画画的起笔位置
    // 后面两个参数是画的时候是否对图片进行拉伸缩放，比如直接拉伸到和窗口一样大
    // 最后一个参数是要画的背景，QPixmap类型
}
```
***


## 绘图设备

Qt的绘图系统实际上是，使用`QPainter`在`QPainterDevice`上进行绘制，它们之间使用`QPainEngine`进行通信

之前的绘图设备就直接使用的是`this`,即Widget类的当前窗口

绘图设备是指继承了QPainterDevice的子类

### QPixmap:

专门为图像在屏幕上的显示做了优化，使图像的显示更清晰

1. 包含头文件：`#include<QPixmap>`
2. 创建绘图设备：`QPixmap pix(x,y);`，指定画布大小
3. 声明画家：`QPainter painter(&pix);`
4. 画图
5. 保存：`pix.save("要保存的路径");`

画完之后不会显示在主窗体中，而是保存在了指定路径的文件中

- 对图片进行缩放：`pix = pix.scaled(pin.width()*0.5, pix.height()*0.5);`
    
### **QBitmap：** 
色深限定为1，即只有黑白两色


### **QImage：** 

专门为图像的像素级访问做了优化，即可以以像素为单位修改图像

1. 包含头文件：`#include<QImage>`
2. 创建绘图设备：`QImage img(int width, int height, QFormat format);`
3. 声明画家
4. 画图
5. 保存：`img.save("路径");`

另一种画图方式
1. 创建画家，直接在窗口上画画：`QPainter painter(this);`
2. 创建img对象：`QImage img;` `img.load("资源路径");`
3. 修改像素点：`img.setPixel(int x, int y,qRgb(255,0,0));`
4. 画图：`painter.drawImage(int x, int y, Qimage img);`

### **QPicture：** 
可以记录和重现QPainter的各条命令

1. 包含头文件:`#include<QPicture>`
2. 创建画布：`QPicture pic`
3. 创建画家对象，可以先只创建不给他画布：`Qpainter painter;`
4. 开始画画：`painter.begin(&pic);`
5. 画画
6. 结束画画：`painter.end();
7. 保存：`pic.save();`

- 重现绘图指令

```C++
QPainter painter(this);

QPicture pic;
pic.load("之前用QPicture保存的文件路径");

painter.drawPicture(x,y,pic);

```