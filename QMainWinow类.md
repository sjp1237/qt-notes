#### QMainWindow类

QMainWindow 是标准基础窗口中结构最复杂的窗口，其组成如下:

- 提供了菜单栏 , 工具栏 , 状态栏 , 停靠窗口
- 菜单栏：只能有一个，位于窗口的最上方
- 工具栏：可以有多个，默认提供了一个，窗口的上下左右都可以停靠
- 状态栏：只能有一个，位于窗口最下方
- 停靠窗口：可以有多个，默认没有提供，窗口的上下左右都可以停靠


![image-20230430151052978](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430151052978.png)





## 菜单栏

### 创建菜单栏

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430191214372.png" alt="image-20230430191214372" style="zoom:67%;" />1

## 在菜单栏里创建菜单

顶级菜单可以在UI界面窗口中双击，直接输入文本即可。

![image-20230430151528213](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430151528213.png)



### 添加菜单项

在ui界面中，我们不能直接在菜单项中添加中文名。

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430152103342.png" alt="image-20230430152103342" style="zoom:67%;" />1

### 方法一

可以在外面创建出QAction对象

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430152005564.png" alt="image-20230430152005564" style="zoom:67%;" />1

添加成功后，直接用鼠标拖拽到对应的菜单项即可。

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430152322594.png" alt="image-20230430152322594" style="zoom: 50%;" />1

## 方法二，代码的方法添加

```c++
// 给菜单栏添加菜单
QAction *QMenuBar::addMenu(QMenu *menu);
QMenu *QMenuBar::addMenu(const QString &title);
QMenu *QMenuBar::addMenu(const QIcon &icon, const QString &title);

// 给菜单对象添加菜单项(QAction)
QAction *QMenu::addAction(const QString &text);
QAction *QMenu::addAction(const QIcon &icon, const QString &text);

// 添加分割线
QAction *QMenu::addSeparator();
```



#### 添加分割线

![image-20230430161432909](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430161432909.png)

### QAction的事件处理

如果要想要点击我们的菜单项能够有反应，则需要设定事件。

在qt中，**点击菜单项**会默认发出一个QAction::triggere信号。

![image-20230430195011584](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430195011584.png)



例子：

点击保存菜单项，显示一个提示保存成功的提示框。

```c++
    connect(ui->save_action,&QAction::triggered,this,[=](){
        QMessageBox::information(this,"提示","保存成功");
    });

```

![image-20230430195314811](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430195314811.png)

### 工具栏

1.添加工具栏

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430200729208.png" alt="image-20230430200729208" style="zoom: 67%;" />1

2.添加工具栏按钮

方式 1：先创建 QAction 对象，然后拖拽到工具栏中，和添加菜单项的方式相同，

注意：同一个 QAction 对象拖拽到工具栏，和拖拽到菜单项中，则属于同一个对象，所以他们的触发的槽函数是一样的。

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430200845277.png" alt="image-20230430200845277" style="zoom:50%;" />1

方式二：直接调用API函数。

```c++
// 在QMainWindow窗口中添加工具栏
void QMainWindow::addToolBar(Qt::ToolBarArea area, QToolBar *toolbar);
void QMainWindow::addToolBar(QToolBar *toolbar);
QToolBar *QMainWindow::addToolBar(const QString &title);

// 将Qt控件放到工具栏中 
// 工具栏类: QToolBar
// 添加的对象只要是QWidget或者启子类都可以被添加
//QPushButton，QLineEdit可以通过这个接口来添加
QAction *QToolBar::addWidget(QWidget *widget);

// 添加QAction对象
QAction *QToolBar::addAction(const QString &text);
QAction *QToolBar::addAction(const QIcon &icon, const QString &text);

// 添加分隔线
QAction *QToolBar::addSeparator();
```

例子：

给工具栏添加保存按钮，搜索按钮，搜索框.

```c++
   ui->mainToolBar->addAction("保存");//添加保存
    ui->mainToolBar->addWidget(new QPushButton("搜索"));
    ui->mainToolBar->addWidget(new QLineEdit());
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430205303651.png" alt="image-20230430205303651" style="zoom: 80%;" />1



#### 工具栏属性值的设置

点击ui界面



<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430210611420.png" style="zoom:80%;" />1



设置工具栏中的按钮显示：

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430211935726.png" alt="image-20230430211935726" style="zoom:80%;" />11



<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430212028101.png" alt="image-20230430212028101" style="zoom: 50%;" />1



### 添加多个工具栏





## 状态栏

状态栏是QStatusBar类型

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430214640914.png" alt="image-20230430214640914" style="zoom:80%;" />1

#### 在状态栏中添加子控件

通过调用api函数给状态栏添加子控件

```c++
//往状态栏中最后一个位置添加控件
void addWidget(QWidget *widget, int stretch = 0)
//往状态栏中index位置上添加控件，0为第一个位置的控件，1为第二个位置，以此类推
int  insertWidget(int index, QWidget *widget, int stretch = 0)

```

例子:

```c++
    ui->statusBar->addWidget(new QPushButton("问题"));
    ui->statusBar->addWidget(new QPushButton("编译输出"));
    //往状态栏中的第一个位置添加控件，添加在 “问题”按钮后面
    ui->statusBar->insertWidget(1,new QPushButton("应用程序输出"));
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430221711281.png" alt="image-20230430221711281" style="zoom:67%;" />



##### 状态栏的槽函数

```c++
//清除文本信息
[slot] void QStatusBar::clearMessage();
//显示文本信息，timeout=0表示的文本信息会一直显示，如果大于0，则为在timeout毫秒后清除文本信息
[slot] void QStatusBar::showMessage(const QString &message, int timeout = 0);
```

例子：

- 状态栏显示的文本信息和子控件是不能同时显示出来的，它们两者会相互覆盖。
- 如果子控件先显示出来，则会覆盖显示信息
- 如果显示信息先显示出来，则会覆盖子控件。

![image-20230430223329977](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430223329977.png)



![image-20230430223459622](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230430223459622.png)

