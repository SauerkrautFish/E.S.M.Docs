# mac环境下django的安装



---

### 下载

从[官方网站][1]下载最新的稳定版本：DJango-1.x.y.tar.gz，

记住是最新的官方版本哦。其中 x.y 是版本号。进入你下载该文件的文件夹目录，执行如下命令:（Mac下默认是/Users/xxx/Downloads，xxx是你的用户名）

    $ tar zxvf Django-1.x.y.tar.gz

也可以从 Github 上下载最新版，地址：https://github.com/django/django：

    git clone https://github.com/django/django.git


如果安装了pip，也可以：

    pip install django

进入解压后的目录：

    cd Django-1.x.y
    sudo python setup.py install

安装成功后会输出以下信息：

    ……
    Processing dependencies for Django==1.x.y
    Finished processing dependencies for Django==1.x.y

再进入我们的站点目录，创建 Django 项目：

    $ django-admin.py startproject testdj

### 启动服务：

    cd testdj # 切换到我们创建的项目
    $ python manage.py runserver
    ……
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.

以上信息说明，项目已启动，访问地址为http://127.0.0.1:8000/。


[1]: https://www.djangoproject.com/download/
