> django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.Did you install mysqlclient?

-  解决：工程目录下,在 init.py中加入以下代码
```
import pymysql
pymysql.install_as_MySQLdb()
```

> __init__() missing 1 required positional argument: 'on_delete'

- Django > 2.0,定义外键和一对一关系的时候需要加on_delete选项，此参数为了避免两个表里的数据不一致问题
* on_delete有CASCADE、PROTECT、SET_NULL、SET_DEFAULT、SET()五个可选择的值
* CASCADE：此值设置，是级联删除。
* PROTECT：此值设置，是会报完整性错误。
* SET_NULL：此值设置，会把外键设置为null，前提是允许为null。
* SET_DEFAULT：此值设置，会把设置为外键的默认值。
* SET()：此值设置，会调用外面的值，可以是一个函数。