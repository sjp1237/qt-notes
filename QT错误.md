##### [QT编译器报错] ‘QObject’ is an inaccrssible base

解决方法: 注意connect的sender和receiver两者对象均要public继承QObject对象, 而不允许[private](https://so.csdn.net/so/search?q=private&spm=1001.2101.3001.7020)(默认)继承.