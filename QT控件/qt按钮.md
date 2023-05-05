### 按钮基类QAbstractButton

> 在 QT 中为我们提**供了可以直接使用的按钮控件**，如下图。这些按钮种类虽然繁多， 但是它们都拥有相同的父类 **QAbstractButton**。父类中的接口，子类中一样



## 标题和图标

```c++
// 参数text的内容显示到按钮上
void QAbstractButton::setText(const QString &text);
// 得到按钮上显示的文本内容, 函数的返回就是
QString QAbstractButton::text() const;

// 得到按钮设置的图标
QIcon icon() const;
// 给按钮设置图标
void setIcon(const QIcon &icon);

// 得到按钮图标大小
QSize iconSize() const
// 设置按钮图标的大小
[slot]void setIconSize(const QSize &size);
```

> - 

#### 信号

> 这些信号都按钮被点击之后发射出来的，只是在细节上有细微的区别，其中最常用的是 clicked(), **通过鼠标的不同瞬间状态可以发射出 pressed() 和 released() 信号**，如果鼠标设置了 check 属性，一般通过 toggled() 信号判断当前按钮是选中状态还是非选中状态。

```c++
/*
当按钮被激活时(即，当鼠标光标在按钮内时按下然后释放)，当键入快捷键时，或者当click()或animateClick()被调用时，这个信号被发出。值得注意的是，如果调用setDown()、setChecked()或toggle()，则不会触发此信号。
*/
[signal] void QAbstractButton::clicked(bool checked = false);
// 在按下按钮的时候发射这个信号
[signal] void QAbstractButton::pressed();
// 在释放这个按钮的时候发射直观信号
[signal] void QAbstractButton::released();

```

#### 槽函数

```c++
// 执行一个动画点击:按钮被立即按下，并在毫秒后释放(默认是100毫秒)。
[slot] void QAbstractButton::animateClick(int msec = 100);
// 执行一次按钮点击, 相当于使用鼠标点击了按钮
[slot] void QAbstractButton::click();

// 参考 1.2 中的函数介绍
[slot] void QAbstractButton::setChecked(bool);
// 设置按钮上图标大小
[slot]void setIconSize(const QSize &size);
// 切换可检查按钮的状态。 checked <==> unchecked
[slot] void QAbstractButton::toggle();
```



#### QPushButton

#### 常用的API

这种类型的按钮是 Qt 按钮中使用频率最高的一个，对这个类进行操作，大部分时候都需要使用它从父类继承过来的那些 API。
在 QPushButton 类中，比较常用的一些 API 函数如下:

```c++
// 构造函数
/*
参数:
    - icon: 按钮上显示的图标
    - text: 按钮上显示的标题
    - parent: 按钮的父对象, 可以不指定
*/
//构造函数
QPushButton::QPushButton(const QIcon &icon, const QString &text, QWidget *parent = nullptr);
QPushButton::QPushButton(const QString &text, QWidget *parent = nullptr);
QPushButton::QPushButton(QWidget *parent = nullptr);

// 一般在对话框窗口中使用, 将按钮设置为默认按钮, 自动关联 Enter 键 
void setDefault(bool);
// 判断按钮是不是默认按钮,如果是默认按钮
bool isDefault() const;

/*
将弹出菜单菜单与此按钮关联起来。这将把按钮变成一个菜单按钮，
在某些样式中会在按钮文本的右边产生一个小三角形。
*/
void QPushButton::setMenu(QMenu *menu);

/*
显示(弹出)相关的弹出菜单。如果没有这样的菜单，这个函数什么也不做。
这个函数直到弹出菜单被用户关闭后才返回。
*/
[slot] void QPushButton::showMenu();
```

### 按钮的使用

我们可以知道，使用 QPushButton 这种类型的按钮，有三种使用方式:

1. **作为普通按钮, 可以显示文本信息和图标**
2. **关联一个菜单, 点击按钮菜单弹出**

具体操作可以参考如下代码:

### 按钮的状态

对应按钮来说，一般有三种常见状态，分别为: Normal, Hover, Pressed。

- **Normal**:  普通状态，没有和鼠标做任何接触
- **Hover**:  悬停状态，鼠标位于按钮之上，但是并未按下
- **Pressed**:  按压状态，鼠标键在按钮上处于按下状态

### 普通按钮

普通按钮：**鼠标在按钮上按下**，**按钮从 Normal 切换到 Pressed 状态**，**鼠标释放，按钮从 Pressed 恢复到 Normal 状态**。

普通按钮常用的几个接口：

```c++
    //normalbtn是在ui界面创建的PushButton
    ui->normalbtn->setText("普通按钮");//设置按钮的文本信息
    ui->normalbtn->setIcon(QIcon(":/picture/3aafeaf768aab477086807b12399189a.jpg"));//设置按钮显示的图片
    ui->normalbtn->setIconSize(QSize(30,30));//设置按钮显示的图片的大小
    connect(ui->normalbtn,&QPushButton::clicked,this,[=](){
          qDebug()<<"点击普通按钮";
    });
```



### check属性的按钮

### 按钮的Check属性

>
> 当我们给按钮设置了 check 属性之后， **第一次鼠标按下和释放后， 按钮处于按压状态，再次鼠标按下和释放后，按钮才能恢复到 Normal 状态****。**具有 check 属性的按钮就相当于一个开关，每点击一次才能实现一次状态的切换**。

```c++
// 每当可检查按钮改变其状态时，就会发出此信号。checked在选中按钮时为true，在未选中按钮时为false。
[signal] void QAbstractButton::toggled(bool checked);
```

接口设置check属性：

```c++
   //将checkbtn按钮对象设置为check属性的按钮
    ui->checkbtn->setCheckable(true);
    connect(ui->checkbtn,&QPushButton::toggled,this,[=](bool checkout){
        if(checkout)
        {
            qDebug()<<"按压状态";
        }else{
            qDebug()<<"释放状态";
        }
    });
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230503214054219.png" alt="image-20230503214054219" style="zoom:67%;" />1

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230503214201528.png" alt="image-20230503214201528" style="zoom:67%;" />1

ui界面设置check属性

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230503221132121.png" alt="image-20230503221132121" style="zoom: 67%;" />

### 菜单按钮

所谓的菜单按钮就是将一个菜单跟按钮关联起来，然后点击按钮可以选择菜单里的内容，并触发对应的

```c++
    ui->menuBtn->setText("你喜欢哪种编程语言");
    //创建菜单
    QMenu* m_menu=new QMenu();
    //设置菜单选项
    QAction* a=m_menu->addAction("c++");
    QAction* b=m_menu->addAction("python");
    QAction* c=m_menu->addAction("java");
    QAction* d=m_menu->addAction("go");
    //设置菜单与按钮关联起来
    ui->menuBtn->setMenu(m_menu);
    //设置选择对应的菜单选项 促发的槽函数
    connect(a,&QAction::triggered,this,[=](){
        qDebug()<<"喜欢c++";
    });

    connect(b,&QAction::triggered,this,[=](){
        qDebug()<<"喜欢python";
    });

    connect(c,&QAction::triggered,this,[=](){
        qDebug()<<"喜欢java";
    });

    connect(d,&QAction::triggered,this,[=](){
        qDebug()<<"喜欢go";
    });

```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230503220951622.png" alt="image-20230503220951622" style="zoom: 67%;" />1

### QToolButton

> 这个类也是一个常用按钮类，使用方法和功能跟 QPushButton 基本一致，只不过在对于关联菜单这个功能点上，QToolButton 类可以设置弹出的菜单的属性，以及在显示图标的时候可以设置更多的样式，可以理解为是一个增强版的 QPushButton。
>

```c++
///////////////////////////// 构造函数 /////////////////////////////
QToolButton::QToolButton(QWidget *parent = nullptr);

/////////////////////////// 公共成员函数 ///////////////////////////
/*
    1. 将给定的菜单与此工具按钮相关联。
    2. 菜单将根据按钮的弹出模式显示。
    3. 菜单的所有权没有转移到“工具”按钮(不能建立父子关系)
*/
void QToolButton::setMenu(QMenu *menu);
// 返回关联的菜单，如果没有定义菜单，则返回nullptr。
QMenu *QToolButton::menu() const;

/*
弹出菜单的弹出模式是一个枚举类型: QToolButton::ToolButtonPopupMode, 取值如下:
    - QToolButton::DelayedPopup: 
        - 延时弹出, 按压工具按钮一段时间后才能弹出, 比如:浏览器的返回按钮
        - 长按按钮菜单弹出, 但是按钮的 clicked 信号不会被发射
    - QToolButton::MenuButtonPopup: 
        - 在这种模式下，工具按钮会显示一个特殊的箭头，表示有菜单。
	- 当按下按钮的箭头部分时，将显示菜单。按下按钮部分发射 clicked 信号
    - QToolButton::InstantPopup: 
        - 当按下工具按钮时，菜单立即显示出来。
        - 在这种模式下，按钮本身的动作不会被触发(不会发射clicked信号
*/
// 设置弹出菜单的弹出方式
void setPopupMode(QToolButton::ToolButtonPopupMode mode);
// 获取弹出菜单的弹出方式
QToolButton::ToolButtonPopupMode popupMode() const;

/*
QToolButton可以帮助我们在按钮上绘制箭头图标, 是一个枚举类型, 取值如下: 
    - Qt::NoArrow: 没有箭头
    - Qt::UpArrow: 箭头向上
    - Qt::DownArrow: 箭头向下
    - Qt::LeftArrow: 箭头向左
    - Qt::RightArrow: 箭头向右
*/
// 显示一个箭头作为QToolButton的图标。默认情况下，这个属性被设置为Qt::NoArrow。
void setArrowType(Qt::ArrowType type);
// 获取工具按钮上显示的箭头图标样式
Qt::ArrowType arrowType() const;

```


