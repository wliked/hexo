---
title: python manage.py makemigrations 提示No changes detected的解决方法
date: 2018-02-08 13:46:50
tags: 踩坑
category: experience
---

python manage.py makemigrations 提示No changes detected的解决方案一般是按如下方法进行操作:

<!--more-->

1. 可能是setttings.py 中的INSTALLED_APPS 中没有添加相应的app. 先要保证这里配置了相应的app.

2. 可能是app 下面的migrations文件夹被删除了, 解决方法如下:
   ```python
   python manage.py makemigrations --empty yourappname # 生成一个migrations文件夹和一个空的initial.py
   python manage.py makemigrations # 生成原先的model对应的migration file
   python manage.py migrate --fake # 将原先的model进行一个假执行到数据库(告诉django数据库这些老的数据已经在数据库中了). 这样django会认为之前的model都已经在数据库中了.

   ```

如果要继续修改model并生成对应sql, 可以参考如下的步骤:
>1. 更改你的app下面的model
>2. ```python
    python manage.py makemigrations # 生成新更改的model对应的migration file如003_auto_2018-12-30.py
    python manage.py sqlmigrate yourappname 003 # 生成新的migration file对应的sql
    (注意:这种方式会把model中设置的default值去掉. 生成sql语句后,要手动把这些sql语句中drop default的语句删除)
    ```
