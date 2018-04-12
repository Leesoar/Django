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
        'USER': 'postgre',              # 数据库的user
        'PASSWORD': '',                 # 数据库密码
        'HOST': '127.0.0.1',            # 主机默认值
        'PORT': '5432',                 # 端口默认值
    }
}
```