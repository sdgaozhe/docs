> django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.Did you install mysqlclient?

-  解决：工程目录下,在 init.py中加入以下代码
```
import pymysql
pymysql.install_as_MySQLdb()
```