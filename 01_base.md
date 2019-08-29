
所有控件均是窗口，直接或间接继承自QWidget
--------
QWidget 窗口
QPUshButton 间接继承自 QWidget，所以也是窗口

```
QWidget w;

QPushButtion button;
button.setText("点击");
// 为按扭设置父窗口，如果不设，则按扭单独在一个占新窗口打开。没有父窗口的窗口本身即为父窗口（顶级窗口）
button.setParent(&w);
button.show();

// 设置组件位置与大小 x,y,w,h
button.setGeometry(30, 30, 100, 30);

w.show();
w.setWindowTitle("我的第一个窗口程序");

//点击按扭关闭窗口
QObject::connect(&button, SIGNAL(clicked()), &w, SLOT(close()));

```

信号与巢
----------
```
//点击按扭关闭窗口
QObject::connect(&buttion, SIGNAL(clicked()), &w, SLOT(close()));
```


密码框
-----------
```
QLineEdit *edit = new QLineEdit(this);
// 设置输入提示
edit->setPlaceholderText("please input passwd");
// 输入框更改密码输入框
edit->setEchoMode(QLineEdit::Password);
edit->show();
```



输入提示
------------
```
QLineEdit *edit = new QLineEdit(this);
// 设置输入提示
edit->setPlaceholderText("please input passwd");

QCompleter completer(QStringList() << "aab" << "123" << "998");
completer.setFilterMode(Qt::MatchContains);
edit->setCompleter(&completer);
edit->show();
```



布局
----------
注意：layout不是窗体，他不继承QWdeget，而继承自QObject

// 水平布局
#include <QHBoxLayout>
// 垂直布局
#include <QVBoxLayout>
// 网络布局
#include <QGridLayout>

```
QHBoxLayout  *layout = new QHBoxLayout();
layout.addWidget(button1);
//layout.addWidget(button1, 1);
// 添加一个间距
layout.addSpacing(50);
layout.addWidget(button2);
// 添加一个弹簧
layout.addStretch(1);

w.setLayout(layout);
w.show();
```


