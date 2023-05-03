## QMessageBox

QMessageBox是QDialog的子类，QMessage是一个消息提示框。

只研究类的静态方法。

```c++
// 显示一个模态对话框, 只能
[static] void QMessageBox::about(QWidget *parent, const QString &title, const QString &text);
// 显示一个错误模态对话框

//buttons指定显示什么样的按钮
//defaultButton选择默认按钮，当回车键按下去，则会选择该按钮

//QMessageBox::StandardButton
//返回值跟buttons类型是相同的，都是
[static] QMessageBox::StandardButton QMessageBox::critical(
           QWidget *parent, const QString &title, 
           const QString &text, 
           QMessageBox::StandardButtons buttons = Ok,
           QMessageBox::StandardButton defaultButton = NoButton);
// 显示一个问题模态对话框
[static] QMessageBox::StandardButton QMessageBox::question(
           QWidget *parent, const QString &title, 
           const QString &text, 
           QMessageBox::StandardButtons buttons = StandardButtons(Yes | No), 
           QMessageBox::StandardButton defaultButton = NoButton);

// 显示一个警告模态对话框
[static] QMessageBox::StandardButton QMessageBox::warning(
           QWidget *parent, const QString &title, 
           const QString &text, 
           QMessageBox::StandardButtons buttons = Ok,
           QMessageBox::StandardButton defaultButton = NoButton);

// 显示一个信息模态对话框
[static] QMessageBox::StandardButton QMessageBox::information(
           QWidget *parent, const QString &title, 
           const QString &text, 
           QMessageBox::StandardButtons buttons = Ok,
           QMessageBox::StandardButton defaultButton = NoButton);
```

QMessageBox::StandardButton

![image-20230427220730613](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230427220730613.png)

### about框

```c++

void Dialog::on_about_clicked()
{
    QMessageBox::about(this,"about","about 框");
}

```

![image-20230428112232006](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428112232006.png)

#### 信息提示框

```c++
//当调用这个接口，就会显示一个信息提示框
QMessageBox::information(this,"提示","这是一个信息提示框....");
```

![image-20230427221338394](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230427221338394.png)

#### 问题提示框

```c++
void Dialog::on_question_clicked()
{
   QMessageBox::StandardButton ret=QMessageBox::question(this,"问题","你要保存对应的文件 吗",QMessageBox::Save|QMessageBox::Cancel);
   if(ret==QMessageBox::Save){
       qDebug()<<"保存成功";
   }
   if(ret==QMessageBox::Cancel){
       qDebug()<<"取消保存";
   }
}
```

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230427221413874.png" alt="image-20230427221413874" style="zoom:67%;" />

点击Save，则会打印保存。

#### 警告框

```c++
void Dialog::on_warning_clicked()
{
    QMessageBox::warning(this,"警告","警告....");
}
```

![image-20230428111800384](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428111800384.png)1



