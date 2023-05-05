### Json类

QJsonDocument :它封装了一个完整的 JSON 文档，并且可以从 UTF-8 编码的基于文本的表示以及 Qt 自己的二进制格式读取和写入该文档。

QJsonArray: JSON 数组是一个值列表。可以通过从数组中插入和删除 QJsonValue 来操作该列表。

QJsonObject: JSON 对象是键值对的列表，其中**键用字符串来表示，值由 QJsonValue 表示**。

QJsonValue: 该类封装了 JSON 支持的数据类型。



#### QJsonValue

QJsonValue表示的Json键值对中的 值的对象，在Qt中QJsonValue可以封装的基础类型有

```c++
// Json对象
QJsonValue(const QJsonObject &o);
// Json数组
QJsonValue(const QJsonArray &a);
// 字符串
QJsonValue(const char *s);
QJsonValue(QLatin1String s);
QJsonValue(const QString &s);
// 整形 and 浮点型
QJsonValue(qint64 v);
QJsonValue(int v);
QJsonValue(double v);
// 布尔类型
QJsonValue(bool b);
// 空值类型
QJsonValue(QJsonValue::Type type = Null);
```

如果我们得到一个QJsonValue对象，如何判断是什么类型：

```c++
// 是否是Json数组
bool isArray() const;
// 是否是Json对象
bool isObject() const;
// 是否是布尔类型
bool isBool() const;
// 是否是浮点类型(整形也是通过该函数判断)
bool isDouble() const;
// 是否是空值类型
bool isNull() const;
// 是否是字符串类型
bool isString() const;
// 是否是未定义类型(无法识别的类型)
bool isUndefined() const;
```

通过判断函数得到对象内部数据的实际类型之后，如果有需求就可以再次将其转换为对应的基础数据类型，对应的 API 函数如下：

```c++
// 转换为Json数组
QJsonArray toArray(const QJsonArray &defaultValue) const;
QJsonArray toArray() const;
// 转换为布尔类型
bool toBool(bool defaultValue = false) const;
// 转换为浮点类型
double toDouble(double defaultValue = 0) const;
// 转换为整形
int toInt(int defaultValue = 0) const;
// 转换为Json对象
QJsonObject toObject(const QJsonObject &defaultValue) const;
QJsonObject toObject() const;
// 转换为字符串类型
QString toString() const;
QString toString(const QString &defaultValue) const;
```

#### 2.QJsonObject

QJsonObject 封装了 Json 中的对象，在里边可以存储多个键值对，为了方便操作，键值为字符串类型，值为 QJsonValue 类型。关于这个类的使用类似于 C++ 中的 STL 类，仔细阅读 API 文档即可熟练上手使用，下面介绍一些常用 API 函数:

QJsonObject的构造函数:

```c++
QJsonObject::QJsonObject();	// 构造空对象
//往QJsonObject中插入 key-value构建
iterator QJsonObject::insert(const QString &key, const QJsonValue &value);
```

获取键值对的个数：

```c++
int QJsonObject::count() const;
int QJsonObject::size() const;
int QJsonObject::length() const;
```

删除键值对

```c++
void QJsonObject::remove(const QString &key);
QJsonValue QJsonObject::take(const QString &key);	// 返回key对应的value值
```

通过key查找

```c
iterator QJsonObject::find(const QString &key);
bool QJsonObject::contains(const QString &key) const;
```

遍历：

#### QJsonArray

QJsonArray 封装了 Json 中的数组，在里边可以存储多个元素，QT为了方便操作，所有的元素类统一为 QJsonValue 类型.

创建空的Json数组：

```c++
QJsonArray::QJsonArray();
```

添加数据:

```c++

void QJsonArray::insert(int i, const QJsonValue &value); // 插入到 i 的位置之前
iterator QJsonArray::insert(iterator before, const QJsonValue &value);

void QJsonArray::append(const QJsonValue &value);	// 在尾部追加
void QJsonArray::prepend(const QJsonValue &value); // 添加到数组头部
void QJsonArray::push_back(const QJsonValue &value); // 添加到尾部
void QJsonArray::push_front(const QJsonValue &value); // 添加到头部

```

##### 计算数组中的个数

```c++
int QJsonArray::count() const;
int QJsonArray::size() const;
```

##### 从数组中取出某一个元素的值

```c++
QJsonValue QJsonArray::at(int i) const;
QJsonValue QJsonArray::first() const; // 头部元素
QJsonValue QJsonArray::last() const; // 尾部元素
QJsonValueRef QJsonArray::operator[](int i);
```

##### 删除数组中某一个元素

```c++
iterator QJsonArray::erase(iterator it);    // 基于迭代器删除
void QJsonArray::pop_back();           // 删除尾部
void QJsonArray::pop_front();          // 删除头部
void QJsonArray::removeAt(int i);      // 删除i位置的元素
void QJsonArray::removeFirst();        // 删除头部
void QJsonArray::removeLast();         // 删除尾部
QJsonValue QJsonArray::takeAt(int i);  // 删除i位置的原始, 并返回删除的元素的值
```

##### Json数组的遍历，常用的方式有两种：

```c++
//先得到对象中所有的键值，在遍历键值列表，通过 key 得到 value 值
QStringList QJsonObject::keys() const;
for(int i)
```



#### QJsonDocument

它封装了一个完整的 JSON 文档，并且可以从 UTF-8 编码的基于文本的表示以及 Qt 自己的二进制格式读取和写入该文档,**QJsonObject 和 QJsonArray** **这两个对象中的数据是不能直接转换为字符串类型的**，如果要进行数据传输或者数据的持久化，操作的都是字符串类型而不是QJsonObject或者OJsonArray类型，需要通过一个QJsonDocument类来进行二者切换。

##### 1.QJsonObject 或者 QJsonArray 对象转换为 字符串

##### 1.创建QJsonDocument对象

```c++
QJsonDocument::QJsonDocument(const QJsonObject &object);
QJsonDocument::QJsonDocument(const QJsonArray &array);

```

可以看出，通过构造函数就可以将实例化之后的 **QJsonObject 或者 QJsonArray ** 转换为 **QJsonDocument** 对象了。

##### 2..使用得到的字符串进行数据传输，或者磁盘文件持久化

```c++
// json字符串的二进制数据
QByteArray QJsonDocument::toBinaryData() const;	 
// json字符串的文本格式
QByteArray QJsonDocument::toJson(JsonFormat format = Indented) const;	
```



### 将Json格式的字符串==》QJsonObject或者QJsonArray

一般情况下，通过**网络通信或者读磁盘文件就会得到一个 Json 格式的字符串**，如果想要得到相关的原始数据就需要对字符串中的数据进行解析，具体解析流程如下：

##### 1.将得到的Json格式字符串通过QJsonDocument 类的静态函数转换为QJsonDocument 类对象。

```c++
//将二进制的数据转换为Json字符串
[static] QJsonDocument QJsonDocument::fromBinaryData(const QByteArray &data, DataValidation validation = Validate);
// 参数文件格式的json字符串
[static] QJsonDocument QJsonDocument::fromJson(const QByteArray &json, QJsonParseError *error = Q_NULLPTR);
```

##### 2.将文档对象转换为Json数组/对象

```c++
// 判断文档对象中存储的数据是不是数组
bool QJsonDocument::isArray() const;
// 判断文档对象中存储的数据是不是json对象
bool QJsonDocument::isObject() const
    
// 文档对象中的数据转换为json对象
QJsonObject QJsonDocument::object() const;
// 文档对象中的数据转换为json数组
QJsonArray QJsonDocument::array() const;
```

##### 3.通过调用QJsonArray,QJsonObject类提供的API读出存储在对象中的数据





#### 例子：

1.将如下文本写入到文件中：

```c++
/*
  {
    "name":"张三",
    "age":18,
    "salary":19999.8
    "famliy":{
     "father":"李四",
     "mather":"王五",
     "bother":["田七","赵六"]
    }
    "isAlive":true
}
*/
void QJson_serilization()
{
    //定义一个Json对象
    QJsonObject object;
    object.insert("name",QJsonValue("张三"));
    object.insert("age",QJsonValue("18"));
    object.insert("salary",QJsonValue("1999.8"));

    QJsonObject sub_object;
    sub_object.insert("father","李四");
    sub_object.insert("mather","王五");
    //构建family数组
    QJsonArray array;
    array.push_back("田七");
    array.push_back("赵六");
    
    sub_object.insert("bother",array);
    object.insert("family",sub_object);
    object.insert("isAlive",true);

    QJsonDocument document(object);
    //将document文件转换为Json字符串
    QByteArray data=document.toJson();
    //创建一个文件对象
    QFile f("C:\\Users\\A\\Documents\\10_test\\test");
    f.open(QFile::WriteOnly);
    f.write(data);
    f.close();
}
```

运行结果：

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230505161856691.png" alt="image-20230505161856691" style="zoom:67%;" />1



2.将test中的文件内容转换为Json对象。

json文件

<img src="C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230505171119152.png" alt="image-20230505171119152" style="zoom:50%;" />

```c++
void QJson_deserialization(){
    QFile f("C:\\Users\\A\\Documents\\10_test\\test");
    f.open(QFile::ReadOnly);
    QByteArray data=f.readAll();
    f.close();
    //将文件json字符串转换为document
    QJsonDocument document=QJsonDocument::fromJson(data);
    //判断document是json对象还是json数组
    if(document.isObject())
    {
        QJsonObject m_object=document.object();
        //遍历Json对象
        QStringList keys=m_object.keys();
        for(int i=0;i<m_object.size();i++){
            QString key=keys[i];
            QJsonValue value=m_object[key];
            if(value.isBool()){
                qDebug()<<key<<": "<<value.toBool();
            }
            else if(value.isNull()){
                qDebug()<<key<<":"<<value;
            }else if(value.isArray()){
                QJsonArray arry=value.toArray();
                qDebug()<<key<<":";
                for(int i=0;i<arry.size();i++){
                    qDebug()<<arry[i].toString();
                }
            }else if(value.isDouble()){
                 qDebug()<<key<<": "<<value.toDouble();
            }else if(value.isObject()){

            }else if(value.isString()){
                qDebug()<<key<<": "<<value.toString();
            }else if(value.isUndefined()){

            }
        }
    }
    else if(document.isArray()){
        //判断是不是Json数组
    }
}
```

运行结果：

![image-20230505171141040](C:\Users\A\AppData\Roaming\Typora\typora-user-images\image-20230505171141040.png)