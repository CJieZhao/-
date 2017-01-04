# QSS
---
## 加载QSS文件
``` 
#include <QApplication>
void QSSHelper::setStyle(const QString &style)
{
    QFile qss(style);
    qss.open(QFile::ReadOnly);
    qApp->setStyleSheet(qss.readAll());
    qss.close();
}
```

## QSS 基本用法

1. 设置按钮
```
QPushButton
{
    color: rgb(170, 255, 0);
}
```
