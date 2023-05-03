### QFileDialog

```c++
/*
通用参数:
  - parent: 当前对话框窗口的父对象也就是父窗口
  - caption: 当前对话框窗口的标题
  - dir: 当前对话框窗口打开的默认目录
  - options: 当前对话框窗口的一些可选项,枚举类型, 一般不需要进行设置, 使用默认值即可
  - filter: 过滤器, 在对话框中只显示满足条件的文件, 可以指定多个过滤器, 使用 ;; 分隔
    - 样式举例: 
	- Images (*.png *.jpg)
	- Images (*.png *.jpg);;Text files (*.txt)
	-"Images (*.png *.xpm *.jpg);;Text files (*.txt);;XML files (*.xml)"
  - selectedFilter: 如果指定了多个过滤器, 通过该参数指定默认使用哪一个, 不指定默认使用第一个过滤器
*/
//静态函数
// 打开一个目录, 得到一个已经存在目录的绝对路径
[static] QString QFileDialog::getExistingDirectory(
                  QWidget *parent = nullptr, 
                  const QString &caption = QString(), 
                  const QString &dir = QString(), 
                  QFileDialog::Options options = ShowDirsOnly);

// 打开一个已经存在的文件, 得到这个文件的绝对路径
[static] QString QFileDialog::getOpenFileName(
    	          QWidget *parent = nullptr, 
    		  const QString &caption = QString(), 
                  const QString &dir = QString(), 
                  const QString &filter = QString(), 
                  QString *selectedFilter = nullptr, 
                  QFileDialog::Options options = Options());

// 打开多个文件, 得到这多个已经文件的绝对路径
[static] QStringList QFileDialog::getOpenFileNames(
    	          QWidget *parent = nullptr, 
                  const QString &caption = QString(), 
                  const QString &dir = QString(), 
                  const QString &filter = QString(), 
                  QString *selectedFilter = nullptr, 
                  QFileDialog::Options options = Options());


// 打开一个目录, 使用这个目录来保存指定的文件，不管文件存不存在，都会返回对应的绝对路径
[static] QString QFileDialog::getSaveFileName(
    		  QWidget *parent = nullptr, 
               const QString &caption = QString(), 
                  const QString &dir = QString(), 
                  const QString &filter = QString(), 
                  QString *selectedFilter = nullptr, 
                  QFileDialog::Options options = Options()

```



##### 打开一个文件夹

```c++
void Dialog::on_pushButton_clicked()
{
    QString s=QFileDialog::getExistingDirectory(this,"目录","C:\\Users");
    QMessageBox::information(this,"提示",s);
}
```

在文件夹下面看到的都是文件夹：

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428180904649.png" alt="image-20230428180904649" style="zoom:67%;" />

打开一个文件

```c++
void Dialog::on_pushButton_2_clicked()
{
    //点击图片，返回文件的绝对路径
    QString s=QFileDialog::getOpenFileName(this,"文件","图片","Image(*.png *.jpg);;Text files (*.txt)");
    QMessageBox::information(this,"提示",s);
}
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428195828013.png" alt="image-20230428195828013" style="zoom:67%;" />

##### 保存内容

```c++
void Dialog::on_save_file_clicked()
{
    QString s=QFileDialog::getSaveFileName(this,"保存","C:\\Users","Text files (*.txt)");
    QFile file(s);
    file.open(QIODevice::WriteOnly);
    file.write("hello world");
}
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428201818477.png" alt="image-20230428201818477" style="zoom: 67%;" />1

返回test1.txt绝对路径后，利用QFile将"hello world"保存进test1.txt文件里

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428201853205.png" alt="image-20230428201853205" style="zoom:67%;" />