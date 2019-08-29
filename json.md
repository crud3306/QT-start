
地址：
-------
https://blog.csdn.net/liang19890820/article/details/52767153



#include <QJsonDocument>
#include <QJsonObject>
#include <QJsonArray>
#include <QByteArray>

拼装json字串
------------
```
先拼装jsonObject 或 jsonArray
QJsonObject obj;
obj.insert("username", QString("zhangsan");
obj.insert("password", QString("123456");
obj.insert("gender", 1);

然后拼装QJsonDocument，再用toJson转成json字符串
QJsonDocument doc(obj);

// QJsonDocument的toJson方法把jsonDoc转换成json字符串
QByteArray json = doc.toJson();
// toJson也可转换成紧凑的字串，即不带换行与缩进的
//QByteArray json = doc.toJson(QJsonDocument::Compact);

qDebug() << json;
```


解析json字会符
---------
```
//假定jsonStr为一个json字符串
QJsonDocument jsonDoc = QJsonDocument::fromJson(jsonStr);
QJsonObject obj = jsonDoc.object();
//或者上面两句缩写成下面一行
QJsonObject obj = QJsonDocument::fromJson(jsonStr).object();

qDebug() << obj.value("username").toString();
```


