####  QRect

> 在 Qt 中使用 **QRect 类来描述一个矩形**，常用的 API 如下:

```c++
// 构造函数
// 构造一个空对象
QRect::QRect();
// 基于左上角坐标, 和右下角坐标构造一个矩形对象
QRect::QRect(const QPoint &topLeft, const QPoint &bottomRight);

//获取坐标信息
//获取左上角
QPoint QRect::topLeft() const;
//获取右上角
QPoint QRect::topRight() const;
//获取右下角
QPoint QRect::bottomLeft() const;
//获取右上角
QPoint QRect::bottomRight() const;

// 返回矩形中心点坐标
QPoint QRect::center() const;

```

各个函数返回对应点坐标如下：

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230426173536393.png" alt="image-20230426173536393" style="zoom:80%;" />

