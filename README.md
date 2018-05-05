# Django

开始学习Django！

[*参考教程*](https://www.zmrenwu.com/post/3/)

# 准备

* **Step 1**：创建虚拟环境
```OS
	python -m virtualenv env_name
```

* **Step 2**：触发/激活虚拟环境
```OS
	cd env_name\Scripts\activate.bat
```

* **Step 3**：创建项目
```OS
	cd env_name
	django-admin startproject project_name
```

* **Step 4**：运行
```OS
	cd project_name
	python manage.py runserver [port](可选-设置端口号)
```

* **Step 5**：创建app
```OS
	cd project_name
	python manage.py startapp app_name
```

* **Step 6**：配置app中的models.py与project中的settings.py

> *settings.py*
```python
INSTALLED_APPS = [
    your_app_name,         # 在此处添加本行
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

> *models.py*
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',        #设置所用数据库，此处使用的是postgresql
        'NAME': 'blog',                 # app名称
        'USER': 'postgres',             # 数据库的user
        'PASSWORD': '',                 # 数据库密码
        'HOST': '127.0.0.1',            # 主机默认值
        'PORT': '5432',                 # 端口默认值
    }
}
```

* **Step 7**：写入(app迁移至)数据库
```OS
	cd project_name
	python manage.py migrate       # 将自带的app迁移至数据库
	python manage.py makemigrations app_name         # 创建app的迁移，并生成记录文件
	python manage.py migrate             # 将刚才创建的app迁移至数据库(自动寻找app中的models.py,然后将元数据中定义的db_table的值当表名建表)
```

* **Step 8**：第一个简单网页

> *views.py*（在app_name中）
```python
from django.shortcuts import render
from django.http import HttpResponse
from datetime import datetime
# Create your views here.
def home(request):
	html = "<html><head>HelloWorld<br/></head><body>现在是北京时间 %s。</body></html>" % datetime.now()
	return HttpResponse(html)
```

> *urls.py*（在project_name/project_name中）
```python
from django.contrib import admin
# from django.conf.urls import url
from django.urls import path, re_path     #re_path为正则表达式用法
from your_app_name import views         #此处是你的app名称
 
urlpatterns = [
    re_path(r'^admin/', admin.site.urls),
    re_path(r'^$', views.home),
]

# 或者这样写
# urlpatterns = [
#     url(r'^admin/', admin.site.urls),
#     url(r'^$', views.home),
# ]
```

* **Step 9**：第一个简单网页的进阶版

	在平常的Django项目中，不会在`views.py`中使用html变量来写HTML页面，因为一个网页的HTML代码通常非常多。

	正确的做法是创建模板，即在app_name目录下新建文件夹`templates`，将网页存储在`templates`目录下。

**Example**

	首先在templates中创建home.html('{{}}'为django的模板变量):

> *home.html*
```html
<html>
	<head>
		<title>{{ title }}</title>
	</head>
	<body>
		{{ content }}
	</body>
</html>
```

	然后修改views.py中的内容:

> *views.py*
```python
from django.shortcuts import render

# Create your views here.
def home(request):
	return render(request, 'home.html', context={
				"title": "HelloWorld",
				"content": "demo"
				})
```
# 正式开始
