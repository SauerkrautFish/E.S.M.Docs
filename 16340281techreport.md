# Django框架下后端数据库的使用

## Django项目创建

cd到代码目录，运行命令：

```
django-admin startproject mysite
```

这行代码将会在当前目录下创建一个 `mysite` 目录，目录结构如下：

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

接下来要用到的是`mysite/settings.py`这是Django 项目的配置文件。包含了 Django 项目设置的 Python 模块。通常，这个配置文件使用 SQLite 作为默认数据库。Python 内置 SQLite，所以无需安装额外东西来使用它。

## Django模型使用

编辑 `mysite/settings.py` 文件前，先设置 [`TIME_ZONE`](https://docs.djangoproject.com/zh-hans/2.2/ref/settings/#std:setting-TIME_ZONE) 为自己的时区。

此外，关注一下文件头部的 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/2.2/ref/settings/#std:setting-INSTALLED_APPS) 设置项。这里包括了会在项目中启用的所有 Django 应用。应用能在多个项目中使用，也可以打包并且发布应用，让别人使用它们。

通常， [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/2.2/ref/settings/#std:setting-INSTALLED_APPS) 默认包括了以下 Django 的自带应用：

- [`django.contrib.admin`](https://docs.djangoproject.com/zh-hans/2.2/ref/contrib/admin/#module-django.contrib.admin) -- 管理员站点， 你很快就会使用它。
- [`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/2.2/topics/auth/#module-django.contrib.auth) -- 认证授权系统。
- [`django.contrib.contenttypes`](https://docs.djangoproject.com/zh-hans/2.2/ref/contrib/contenttypes/#module-django.contrib.contenttypes) -- 内容类型框架。
- [`django.contrib.sessions`](https://docs.djangoproject.com/zh-hans/2.2/topics/http/sessions/#module-django.contrib.sessions) -- 会话框架。
- [`django.contrib.messages`](https://docs.djangoproject.com/zh-hans/2.2/ref/contrib/messages/#module-django.contrib.messages) -- 消息框架。
- [`django.contrib.staticfiles`](https://docs.djangoproject.com/zh-hans/2.2/ref/contrib/staticfiles/#module-django.contrib.staticfiles) -- 管理静态文件的框架。

## 数据库配置

我们在项目的 settings.py 文件中找到 DATABASES 配置项，将其信息修改为：

```
DATABASES = {
'default': {
'ENGINE': 'django.db.backends.mysql',  # 或者使用 mysql.connector.django
'NAME': 'test',
'USER': 'test',
'PASSWORD': 'test123',
'HOST':'localhost',
'PORT':'3306',
}
}
```

## 创建APP

Django规定，如果要使用模型，必须要创建一个app。我们使用以下命令创建一个 TestModel 的 app:

```
django-admin startapp TestModel
```

目录结构如下：

```
mysite
|-- TestModel
|   |-- __init__.py
|   |-- admin.py
|   |-- models.py
|   |-- tests.py
|   `-- views.py
```

然后我们就可以在models.py文件里创建数据库的模型了

接下来在settings.py中找到INSTALLED_APPS这一项，如下：

```
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'TestModel',               # 添加此项
)
```

在命令行中运行：

```
python manage.py migrate   # 创建表结构

python manage.py makemigrations TestModel  # 让 Django 知道我们在我们的模型有一些变更
python manage.py migrate TestModel   # 创建表结构
```

```
Creating tables ...
……
Creating table TestModel_test  #我们自定义的表
……
```

表名组成结构为：应用名_类名（如：TestModel_test）。

**注意：**如果我们没有在models给表设置主键，Django会自动添加一个id作为主键。

## Django Auth模块自带的User模型

Django Auth模块自带User模型所包含字段

- username：用户名

- email: 电子邮件

- password：密码

- first_name：名

- last_name：姓

- is_active: 是否为活跃用户。默认是True

- is_staff: 是否为员工。默认是False

- is_superuser: 是否为管理员。默认是False

- date_joined: 加入日期。系统自动生成。

在建立用户，客户模型时我们可以在用户，客户模型内添加以下代码

```
user = models.OneToOneField(User, on_delete=models.CASCADE)
```

来直接使用user模型，并且可以在这个基础上对其进行扩展。使用的联系为1对1关系。

然后可以使用内置User自带create_user方法创建用户，不需要使用save()，如：

```
user = User.objects.create_user(username=username, password=password, email=email)
```

然后再创建一个客户对象，如：

```
customer = UserProfile(user=user)
customer.save()
```

然后就可以很方便的保存扩展的模型表单了，客户提交了一份表单，实际上是存储在了两份表单里，一个是Django自带的User模型，一个则是扩展的客户customers模型。这样就省下了编写需要注册登录的user模型的功夫了。