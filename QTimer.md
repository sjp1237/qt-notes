在进行**窗口程序的处理过程中**，经常要**周期性的执行某些操作，或者制作一些动画效果，看似比较复杂的问题使用定时器就可以完美的解决这些问题**， Qt 中提供了两种定时器方式一种是使用 Qt 中的事件处理函数这个在后续章节会给大家做细致的讲解，本节主要给大家介绍一下 Qt 中的定时器类 QTimer 的使用方法。

要使用它，只需创建一个 QTimer 类对象，然后调用其 start() 函数开启定时器，此后 QTimer 对象就会周期性的发出 timeout() 信号。我们先来了解一下这个类的相关 API。

### 常用接口函数

```c++
// 构造函数
// 如果指定了父对象, 创建的堆内存可以自动析构
QTimer::QTimer(QObject *parent = nullptr)

[slot] void QTimer::start();
// 启动或重新启动定时器，超时间隔为msec毫秒。
[slot] void QTimer::start(int msec); //start(1000);设置1秒
// 停止定时器。
[slot] void QTimer::stop();

// 如果定时器正在运行，返回true; 否则返回false。
bool QTimer::isActive() const;
```

## signals

这个类的信号只有一个，当定时器超时时，该信号就会被发射出来。给这个信号通过 conect() 关联一个槽函数，就可以在槽函数中处理超时事件了。

```c++
[signal] void QTimer::timeout();
```



### 例子

点击开始按钮，开始显示当前每一秒的时间。

点击按钮->触发timer的start函数—>触发timeout信号—>显示一次当前时间。

```c++
#include<QDebug>
Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
    QTimer* t=new QTimer();
    connect(ui->startButton,&QPushButton::clicked,this,[=](){
        if(t->isActive()){
            //点击关闭，则停止显示
            t->stop();
            ui->startButton->setText("开始");
        }else{
            //点击开始，则开始显示
             t->start(1000);//1秒钟
             ui->startButton->setText("关闭");
        }
    });

    connect(t,&QTimer::timeout,this,[=](){
        QTime curtime=QTime::currentTime();
        QString s=curtime.toString("hh:mm:ss.zzz");
         ui->curtime->setText(s);
    });
}
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230426161250495.png" alt="image-20230426161250495" style="zoom:80%;" />1





### 一次性定时器

```c++
// 其他同名重载函数可以自己查阅帮助文档
/*
功能: 在msec毫秒后发射一次信号, 并且只发射一次
参数:
	- msec:     在msec毫秒后发射信号
	- receiver: 接收信号的对象地址
	- method:   槽函数地址
*/
//在msec毫秒后发射一次信号，并接收该信号
[static] void QTimer::singleShot(
        int msec, const QObject *receiver, 
        PointerToMemberFunction method);


//点击一下，获取当前时间
 connect(ui->startButton2,&QPushButton::clicked,this,[=](){
        QTimer::singleShot(1000,this,[=](){
            QTime curtime=QTime::currentTime();
            QString s=curtime.toString("hh:mm:ss.zzz");
            ui->curtime2->setText(s);
        });
 });
```

