### QWidget

QWidget 类是所有窗口类的父类 (控件类是也属于窗口类), **并且 QWidget 类的父类的 QObject**, 也就意味着所有的窗口类对象只要指定了父对象, 都可以实现内存资源的自动回收。



### 构造函数

```c++
// 构造函数
QWidget::QWidget(QWidget *parent = nullptr, Qt::WindowFlags f = Qt::WindowFlags());

//设置父对象
void QWidget::setParent(QWidget *parent);
void QWidget::setParent(QWidget *parent, Qt::WindowFlags f);
// 获取当前窗口的父对象, 没有父对象返回 nullptr
QWidget *QWidget::parentWidget() const;
```



### 窗口位置

```c++
// 得到相对于当前窗口父窗口的几何信息, 边框也被计算在内
QRect QWidget::frameGeometry() const;
// 得到相对于当前窗口父窗口的几何信息, 不包括边框
const QRect &geometry() const;
// 设置当前窗口的几何信息(位置和尺寸信息), 不包括边框
void setGeometry(int x, int y, int w, int h);
void setGeometry(const QRect &);

// 移动窗口, 重新设置窗口的位置
void move(int x, int y);
void move(const QPoint &);


```

示例：

点击按钮，移动窗口，获取窗口

```c++
void Widget::on_pushButton_clicked()
{
    move(10,10);
}

void Widget::on_pushButton_2_clicked()
{
    QRect rect=this->frameGeometry();
    qDebug()<<"左上角: "<<rect.topLeft()
    <<"右上角: "<<rect.topRight()
    <<"左下角: "<<rect.bottomLeft()
    <<"右下角: "<<rect.bottomLeft()
    <<"宽度:"<<rect.width()
    <<"高度: "<<rect.height();
}

void Widget::on_pushButton_3_clicked()
{
    int x=rand()%500;
    int y=rand()%500;
    int wid=width()+10;
    int higth=height()+10;
    setGeometry(x,y,wid,higth);
}
```



### 基于窗口的菜单栏策略实现

```c++
这种方式是使用 Qt 中 QWidget 类中的右键菜单函数  
setContextMenuPolicy(Qt::CustomContextMenu);
  - Qt::NoContextMenu	     --> 不能实现右键菜单
  - Qt::PreventContextMenu   --> 不能实现右键菜单
  - Qt::DefaultContextMenu   --> 基于事件处理器函数 QWidget::contextMenuEvent() 实现
  - Qt::ActionsContextMenu   --> 添加到当前窗口中所有 QAction 都会作为右键菜单项显示出来
  - Qt::CustomContextMenu    --> 基于 QWidget::customContextMenuRequested() 信号实现
```

```c++
#include<QMenu>
#include<QDebug>
Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
   // 当在窗口中点击鼠标右键, 窗口会发出一个信号: QWidget::customContextMenuRequested()
   // 对应发射出的这个信号, 需要添加一个槽函数, 用来显示右键菜单
    setContextMenuPolicy(Qt::CustomContextMenu);
    connect(this,&QWidget::customContextMenuRequested,this,[=](const QPoint point){
        qDebug()<<"右键";
        QMenu* mu=new QMenu(this);
        mu->addAction("c++");
        mu->addAction("c");
        mu->addAction("java");
        mu->addAction("python");
        mu->exec(QCursor::pos());
    });
}
```



### 实现右键菜单栏

## 菜单项

```c++
QMenu(QWidget *parent = nullptr);
//添加菜单项
QAction *addAction(const QString &text);
QAction *addAction(const QIcon &icon, const QString &text);
//相对于屏幕的坐标显示菜单
exec(QCursor::pos());
```





基于鼠标事件实现和基于窗口的菜单策略实现