* 查看python版本 

> python 

* 安装django 

> pip install django 

> pip install -v django==2.0

* 查看Django版本

> python -m django --version

* 创建项目

> django-admin startproject myweb

* 启动服务

> python manage.py runserver

* 创建功能模块

> python manage.py startapp myapp  

---

* 常用命令

> django-admin.py startproject myweb   #创建项目

> python manage.py startapp myapp   #创建app

> python manage.py runserver   #启动Django中的开发服务器

> python manage.py -h   #帮助文档

> python manage.py <command> [options] #Django命令

---

* 数据库操作

> python manage.py makemigrations #根据模型生成映射关系

> python manage.py migrate #生成表

