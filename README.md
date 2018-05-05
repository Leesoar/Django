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

...

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

> 别忘了安装psycopg2
```python
	pip install psycopg2      #postgresql的一个接口
```

> *models.py*
```python
from django.db import models

# Create your models here.

class Article(models.Model):
	title = models.CharField("标题", max_length=50)
	author = models.CharField("作者", max_length=50)
	created_date = models.DateField("创建日期", auto_now_add=True)     # auto_now_add表示只自动获取时间一次，之后不会改变
	modifyed_date = models.DateField("修改日期", auto_now=True)        # auto_now表示自动获取时间并会覆盖上一次获取的时间
	content = models.TextField("文章内容")
	is_show = models.BooleanField("是否显示文章")

	class Meta:
		db_table = "article"       #创建数据库的article表
	def __str__(self):
		return self.title
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

## 数据库操作
	
	在包含manage.py的目录下运行：

```python
	python manage.py shell
```
### 增数据

> 创建分类和标签
```python
	>>> from app_name.models import Category, Tag, Article      #从应用中导入这三个类
	>>> c = Category(name='im category')    #对分类类进行实例化
	>>> c.save()
	>>> t = Tag(name='im tag')         #对标签类进行实例化
	>>> t.save()
```

> 创建文章
```python
	>>> from app_name.models import Category, Tag, Article
	>>> from django.utils import timezone
	>>> from django.contrib.auth.models import User

	>>> user = User.objects.get(username='raoseel')
	>>> c = Category.objects.get(name='im category')

	>>> a = Article(title='im title', body='im body', created_time=timezone.now(), modified_time=timezone.now(), category=c, author=user)
	>>> a.save()
```

### 查数据
```python
	>>> Category.objects.all()
	>>> Tag.objects.all()
	>>> Article.objects.all()
```

### 改数据
```python
	>>> c = Category.objects.get(name='im category')
	>>> c.name = 'im new category'
	>>> c.save()
	>>> Category.objects.all()
```

### 删数据
```python
	>>> a = Article.objects.get(title='im title')
	>>> a
	<Article: im title>
	>>> a.delete()
	(1, {'com.Article_tags': 0, 'com.Article': 1})
	>>> com.objects.all()
```

## 配置应用

	在app_name中创建urls.py（便于未来管理）：

> *app_name/urls.py*
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

> *views.py*
```python
	from django.shortcuts import render
	from django.http import HttpResponse

	def index(request):
	    return HttpResponse("welcome to leesoar.com!")
```

	Django并不知道我们创建了一个urls.py在app文件夹中，所以我们需要告诉它：

> *project_name/urls.py*
```python
	from django.contrib import admin
	from django.urls import path,include

	urlpatterns = [
	    path('admin/', admin.site.urls),
	    path('', include('com.urls')),
	]
```
### 使用模板系统

	在project文件夹中创建templates,然后在templates文件夹创建app_name文件夹(便于未来维护),再创建index.html：

```html
	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title>{{ title }}</title>      #用 {{ }} 包起来的变量叫做模板变量。Django 在渲染这个模板的时候会根据我们传递给模板的变量替换掉这些变量。最终在模板中显示的将会是我们传递的值。
	</head>
	<body>
	<h1>{{ welcome }}</h1>
	</body>
	</html>
```

	然后告诉Django该去哪里找模板来渲染，在settings.py中修改：
```python
	TEMPLATES = [
	    {
	        ...
	        'DIRS': [os.path.join(BASE_DIR, 'templates')],
	        ...
	    },
	]
```

	修改views.py：
```python
	from django.shortcuts import render

	def index(request):
	    return render(request, 'com/index.html', context={
	                      'title': 'leesoar.com', 
	                      'welcome': 'welcome to leesoar.com!'
	                  })
```

## 创建超级用户
```python
	python manage.py createsuperuser
```