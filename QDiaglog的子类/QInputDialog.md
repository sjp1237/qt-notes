### QInputDialog

> QInputDialog 类是 QDialog 的子类，通过这个类我们可以得到一个输入对话框窗口，根据实际需求我们可以在这个输入窗口中输入整形 , 浮点型 , 字符串类型的数据，并且还可以显示下拉菜单供使用者选择
>



### 

### 浮点数对话框

```c++
// 得到一个可以输入浮点数的对话框窗口, 返回对话框窗口中输入的浮点数
/*
参数:
  - parent: 对话框窗口的父窗口
  - title: 对话框窗口显示的标题信息
  - label: 对话框窗口中显示的文本信息(用于描述对话框的功能)
  - value: 对话框窗口中显示的浮点值, 默认为 0
  - min: 对话框窗口支持显示的最小数值
  - max: 对话框窗口支持显示的最大数值
  - decimals: 浮点数的精度, 默认保留小数点以后1位
  - ok: 传出参数, 用于判断是否得到了有效数据, 一般不会使用该参数
  - flags: 对话框窗口的窗口属性, 使用默认值即可
*/
[static] double QInputDialog::getDouble(
    		QWidget *parent, const QString &title, 
    		const QString &label, double value = 0, 
    		double min = -2147483647, double max = 2147483647, 
    		int decimals = 1, bool *ok = nullptr, 
    		Qt::WindowFlags flags = Qt::WindowFlags());

```

例子：

![image-20230428212719438](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428212719438.png)

输出

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428212744051.png" alt="image-20230428212744051"  />



#### 整形对话框

```c++
[static] int QInputDialog::getInt(
    		QWidget *parent, const QString &title, 
    		const QString &label, int value = 0, 
    		int min = -2147483647, int max = 2147483647, 
    		int step = 1, bool *ok = nullptr, 
    		Qt::WindowFlags flags = Qt::WindowFlags());

```

例子：

![image-20230428213930691](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428213930691.png)

最终选择的结果返回给i;



### Item对话框

```c++
// 得到一个带下来菜单的对话框窗口, 返回选择的菜单项上边的文本信息
/*
参数:
  - parent: 对话框窗口的父窗口
  - title: 对话框窗口显示的标题信息
  - label: 对话框窗口中显示的文本信息(用于描述对话框的功能)
  - items: 字符串列表, 用于初始化窗口中的下拉菜单, 每个字符串对应一个菜单项
  - current: 通过菜单项的索引指定显示下拉菜单中的哪个菜单项, 默认显示第一个(编号为0)
  - editable: 设置菜单项上的文本信息是否可以进行编辑, 默认为true, 即可以编辑
  - ok: 传出参数, 用于判断是否得到了有效数据, 一般不会使用该参数
  - flags: 对话框窗口的窗口属性, 使用默认值即可
  - inputMethodHints: 设置显示模式, 默认没有指定任何特殊显示格式, 显示普通文本字符串
    - 如果有特殊需求, 可以参数帮助文档进行相关设置
*/
[static] QString QInputDialog::getItem(
    		QWidget *parent, const QString &title, 
    		const QString &label, const QStringList &items, 
    		int current = 0, bool editable = true, bool *ok = nullptr, 
    		Qt::WindowFlags flags = Qt::WindowFlags(), 
    		Qt::InputMethodHints inputMethodHints = Qt::ImhNone);
```





![image-20230428215636127](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428215636127.png)



显示：

![image-20230428215711125](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230428215711125.png)



#### 多行对话框

```c++
// 得到一个可以输入多行数据的对话框窗口, 返回用户在窗口中输入的文本信息
/*
参数:
  - parent: 对话框窗口的父窗口
  - title: 对话框窗口显示的标题信息
  - label: 对话框窗口中显示的文本信息(用于描述对话框的功能)
  - text: 指定显示到多行输入框中的文本信息, 默认是空字符串
  - ok: 传出参数, 用于判断是否得到了有效数据, 一般不会使用该参数
  - flags: 对话框窗口的窗口属性, 使用默认值即可
  - inputMethodHints: 设置显示模式, 默认没有指定任何特殊显示格式, 显示普通文本字符串
    - 如果有特殊需求, 可以参数帮助文档进行相关设置
*/



[static] QString QInputDialog::getMultiLineText(
    		QWidget *parent, const QString &title, const QString &label, 
    		const QString &text = QString(), bool *ok = nullptr, 
    		Qt::WindowFlags flags = Qt::WindowFlags(), 
    		Qt::InputMethodHints inputMethodHints = Qt::ImhNone);


。
```

![image-20230429112220837](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429112220837.png)

![image-20230429111645383](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429111645383.png)

### 单行对话框

```c++
/*
参数:
  - parent: 对话框窗口的父窗口 
  - title: 对话框窗口显示的标题信息
  - label: 对话框窗口中显示的文本信息(用于描述对话框的功能)
  - mode: 指定单行编辑框中数据的反馈模式, 是一个 QLineEdit::EchoMode 类型的枚举值
    - QLineEdit::Normal: 显示输入的字符。这是默认值
    - QLineEdit::NoEcho: 不要展示任何东西。这可能适用于连密码长度都应该保密的密码。
    - QLineEdit::Password: 显示与平台相关的密码掩码字符，而不是实际输入的字符。
    - QLineEdit::PasswordEchoOnEdit: 在编辑时按输入显示字符，否则按密码显示字符。
  - text: 指定显示到单行输入框中的文本信息, 默认是空字符串
  - ok: 传出参数, 用于判断是否得到了有效数据, 一般不会使用该参数
  - flags: 对话框窗口的窗口属性, 使用默认值即可
  - inputMethodHints: 设置显示模式, 默认没有指定任何特殊显示格式, 显示普通文本字符串
     - 如果有特殊需求, 可以参数帮助文档进行相关设置
*/
[static] QString QInputDialog::getText(
    		QWidget *parent, const QString &title, const QString &label,
    		QLineEdit::EchoMode mode = QLineEdit::Normal, 
    		const QString &text = QString(), bool *ok = nullptr, 
    		Qt::WindowFlags flags = Qt::WindowFlags(), 
    		Qt::InputMethodHints inputMethodHints = Qt::ImhNone);

```



##### 密码框

![image-20230429113052301](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429113052301.png)

![image-20230429113008542](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429113008542.png)

#### 不展示任何信息

![image-20230429120318960](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429120318960.png)

即使输入了字符，但是也没显示出来

![image-20230429113136883](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230429113136883.png)