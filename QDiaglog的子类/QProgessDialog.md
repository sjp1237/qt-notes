

## QProgessDialog

```c++
// 构造函数
/*
参数:
  - labelText: 对话框中显示的提示信息
  - cancelButtonText: 取消按钮上显示的文本信息
  - minimum: 进度条最小值
  - maximum: 进度条最大值
  - parent: 当前窗口的父对象
  - f: 当前进度窗口的flag属性, 使用默认属性即可, 无需设置
*/

//构造函数
QProgressDialog::QProgressDialog(
	QWidget *parent = nullptr, 
	Qt::WindowFlags f = Qt::WindowFlags());


QProgressDialog::QProgressDialog(
	const QString &labelText, const QString &cancelButtonText, 
	int minimum, int maximum, QWidget *parent = nullptr,
	Qt::WindowFlags f = Qt::WindowFlags());


// 设置取消按钮显示的文本信息
[slot] void QProgressDialog::setCancelButtonText(const QString &cancelButtonText);

// 公共成员函数和槽函数
QString QProgressDialog::labelText() const;
void QProgressDialog::setLabelText(const QString &text);

// 得到进度条最小值
int QProgressDialog::minimum() const;
// 设置进度条最小值
void QProgressDialog::setMinimum(int minimum);
// 得到进度条最大值
int QProgressDialog::maximum() const;
// 设置进度条最大值
void QProgressDialog::setMaximum(int maximum);
// 设置进度条范围(最大和最小值)
[slot] void QProgressDialog::setRange(int minimum, int maximum);
// 得到进度条当前的值
int QProgressDialog::value() const;
// 设置进度条当前的值
void QProgressDialog::setValue(int progress);

//当进度条走到100%时是否自动调整为0%，close默认为true
bool QProgressDialog::autoReset() const;
// 当value() = maximum()时，进程对话框是否调用reset()，此属性默认为true。
void QProgressDialog::setAutoReset(bool reset);

//当进度条走到100%时是否自动关闭，close默认为true
bool QProgressDialog::autoClose() const;
// 当value() = maximum()时，进程对话框是否调用reset()并且隐藏，此属性默认为true。
void QProgressDialog::setAutoClose(bool close);

// 判断用户是否按下了取消键, 按下了返回true, 否则返回false
bool wasCanceled() const;


/*
参数:
	Qt::NonModal  -> 非模态
	Qt::WindowModal	-> 模态, 阻塞父窗口
	Qt::ApplicationModal -> 模态, 阻塞应用程序中的所有窗口
*/
void QWidget::setWindowModality(Qt::WindowModality windowModality);
```





### 例子：

- 基于定时器模拟文件拷贝的场景
- 点击按钮，同时启动定时器和窗口显示。
- 通过定时器信号，按照固定频率更新对话框进度条
- 当进度条==最大值，关闭定时器，并且析构进度对话框。

```c++
void MainWindow::on_pushButton_3_clicked()
{
    QProgressDialog* progress=new QProgressDialog("正在拷贝数据","取消保存",0,1000,this);
    progress->setWindowTitle("请稍后....");//设置对话框的标题
    progress->setWindowModality(Qt::WindowModal);//设置为模态对话框
    progress->show();

    static int i=0;//设置为静态变量，防止函数出了作用域，i变量被销毁
    QTimer* t=new QTimer();
    connect(t,&QTimer::timeout,progress,[=](){
       progress->setValue(i);
       i++;
       if(i>=progress->maximum()){
           //当i等于最大值时，定时器停止，并销毁该对话框，和定时器。
           t->stop();
           i=0;
           delete progress;
           delete t;
       }
    });
    t->start(5);
}
```





![image-20230429221302881](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429221302881.png)