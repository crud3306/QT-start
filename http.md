

参考地址：
--------------
https://blog.csdn.net/liang19890820/article/details/52535755 (http请求)
https://blog.csdn.net/liang19890820/article/details/52767153 (json处理)


需要用到的类
---------
QNetworkAccessManager
QNetworkRequest
QUrl



```
// URL
QString baseUrl = "http://xxxxxx";

// 构造请求
QNetworkRequest request;
request.setUrl(QUrl(baseUrl));

QNetworkAccessManager *manager = new QNetworkAccessManager(this);
// 发送请求
QNetworkReply *pReplay = manager->get(request);

// 开启一个局部的事件循环，等待响应结束，退出
QEventLoop eventLoop;
QObject::connect(manager, &QNetworkAccessManager::finished, &eventLoop, &QEventLoop::quit);
eventLoop.exec();

// 获取响应信息
QByteArray bytes = pReplay->readAll();
qInfo() << bytes;
```


get请求
------------
```
#include <QNetworkAccessManager>
#include <QNetworkRequest>
#include <QNetworkReply>
#include <QJsonDocument>
#include <QJsonObject>


QNetworkRequest req;
req.setUrl(QUrl("http://xxxx:8080/sso/authority/login"));

QNetworkAccessManager *manager = new QNetworkAccessManager(this);
manager->get(req);

connect(manager, &QNetworkAccessManager::finished, [](QNetworkReply* reply){
    if (reply->error() != QNetworkReply::NoError) {
        qDebug() << "Error:" << reply->errorString();
    } else {
        QByteArray buf = reply->readAll();
        qDebug() << "response:" << buf;

        QJsonDocument doc = QJsonDocument::fromJson(buf);
        QJsonObject obj = doc.object();
        //QInternal errorNo = obj.value("error");
        QString errmsg = obj.value("errmsg").toString();
        qDebug() << "response:" << errmsg.toUtf8().data();
    }
});
```


post请求
------------
```
#include <QNetworkAccessManager>
#include <QNetworkRequest>
#include <QNetworkReply>
#include <QJsonDocument>
#include <QJsonObject>


QNetworkRequest req;
req.setUrl(QUrl("http://xxxx:8081/passport/login"));
//req.setHeader(QNetworkRequest::ContentTypeHeader, "application/x-www-form-urlencoded");

QJsonObject obj;
obj.insert("user_name", QString("q20190501"));
obj.insert("password", QString("aaaaaa"));

QNetworkAccessManager *manager = new QNetworkAccessManager(this);
manager->post(req, QJsonDocument(obj).toJson());

connect(manager, &QNetworkAccessManager::finished, [](QNetworkReply* reply){
    if (reply->error() != QNetworkReply::NoError) {
        qDebug() << "Error:" << reply->errorString();
    } else {
        QByteArray buf = reply->readAll();
        qDebug() << "response:" << buf;

        QJsonDocument doc = QJsonDocument::fromJson(buf);
        QJsonObject obj = doc.object();

        QJsonObject resStatusData = obj.value("status").toObject();
        int errorNo = resStatusData.value("code").toInt();
        QString errmsg = resStatusData.value("msg").toString();
        qDebug() << "response: code " << errorNo <<  "; msg " << errmsg.toUtf8().data();
    }
});
```



