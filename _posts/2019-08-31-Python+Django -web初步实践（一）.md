---
layout:     post
title:      Python+Django -web初步实践（一）
subtitle:   Python+Django
date:       2019-8-31
author:     kkkkkuboy
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - github
---

# Python+Django -web初步实践（一）

## Django

Django是一个高级Python Web框架，它能够帮助开发人员快速进行web开发。由经验丰富的开发人员构建，它需要处理Web开发的许多麻烦，所以您可以专注于编写应用程序，而无需重新发明轮子。它是免费的，开源的。

Django项目与应用

- 一个Django项目就是一个基于Django的Web应用。
- 一个Django项目中包含一组配置和若干个Django应用。
- 一个Django应用就是一个可重用的Python软件包，提供一定的功能。
- 一个Django应用中可以包含models, views, templates, template tags, static files, URLs等。
- 一个Django项目可以包含多个Django应用。
- 一个Django应用也可以被包含到多个Django项目中，因为Django应用是可重用的Python软件包。
  

## 环境

**Windows+python**3.7

**IDE：pycharm**

## 创建项目

### 操作步骤

#### 建立虚拟环境

**为什么要建立虚拟环境**？

1. 可以隔离项目之间的第三方包依赖，如A项目依赖django1.2.5，B项目依赖django1.3。
2. 为部署应用提供方便，把开发环境的虚拟环境打包到生产环境即可,不需要在服务器上再折腾一翻。在服务器上都不用安装virtualenv，直接将virtualenv创建的目录拷贝到服务器，修改路径，进行虚拟环境迁移就可以用了。
3. 还可以用在没有root权限的python环境配置上，如果没有root权限，可以先自己搞一个virtualenv，再在virtualenv中使用pip安装。（系统中没有pip，并且也没有root权限使用sudo apt-get安装）
   

1）直接在pycharm创建Django项目，在创建过程中选择或新建该项目的虚拟环境，然后在setting中选择虚拟环境

2）或者通过命令行

```bash
#先创建一个文件夹，若没有安装virtualenv不能使用venv模块，安装
> pip install --user virtualenv

#切换到创建的目录，创建一个虚拟环境
> python -m venv 虚拟环境名

#激活虚拟环境，在命令行提示符中输入
> 虚拟环境名\Scripts\activate
#会进入虚拟环境，前面会有一个"(虚拟环境名)路径>"
#换做deactivate会停止

#在虚拟环境下，安装django并在其中创建项目
> pip install Django
#注意若同时安装了python3和python3自己的path设置，我是使用python3和pip3

#在虚拟环境下创建一个新项目
> django-admin startproject 项目名
```

使用命令行创建虚拟环境会生成一个虚拟环境的文件夹，直接在pycharm创建的项目会在用户目录下创建一个虚拟环境文件夹，他的文件名师在创建是自己命名的，便于以后重复使用某一虚拟环境，不用再新建

创建的虚拟环境文件结构主要有

- Include
- Lib
- Scripts

我们在虚拟环境运行`python manage.py runserver`可以指定端口，在127.0.0.1:端口号 可以看到运行成功

#### 数据迁移

```bash
> python manage.py migrate
```



### 目录说明

新建一个Django项目后，生成一个项目名称的文件夹和一个manage.py管理程序

#### manage.py

与项目进行交互的命令行工具集入口，接受命令并交给django相关部分去执行

实际上就是项目管理器

```bash
> python manage.py
#查看命令集
```



#### 项目文件夹

项目的文件夹，包含项目最基本的一些配置

- __ init__ .py

- setting.py

  *Django如何与你系统交互以及如何管理项目，里包括有各种配置设置*

- urls.py

  *Django项目中所有地址（页面）都需要我们自己去配置其URL，来告诉Django创建哪些网页来响应浏览器请求*

- wsgi.py

  *WSGI(Python Web Server Gateway Interface)*

  *是python应用于Web服务器之间的接口*



## 创建应用程序

### 操作步骤

命令行下输入(虚拟环境下)

```bash
> python manage.py startapp app名
```

生成一个app文件夹

我们需要**将我们的app名添加到项目settings.py文件中的INSTALLED_APPS中**，才能使用

### 目录说明

在创建的应用目录下会产生以下几个文件

- migrations文件目录

  通常是数据迁移文件，每一次执行makemigrations并对数据库进行改变时会生成一个类似记录建表以及相应改变的命令文件。

  在models.py模型类和数据库表之间迁移

- admin.py

  这个文件中可以自定义django管理工具，比如设置在管理界面能够管理的项目，或者通过重新定义与系统管理有关的类对象，向管理功能增加新的内容。

  向管理网站注册模型，即管理我们应用的models里所建立的模型时，需要再admin.py文件中添加。

- apps.py

- models.py

  这是应用的数据类型，每个django应用都应当有一个 modles.py文件，虽然该文件可以为空，但不宜删除。在文件中定义的类相当于一个数据表，类内的每个属性相当于一个字段。

- tests.py

- views.py

  是一个重要的文件，用户保存响应各种请求的函数或者类。如果编写的是函数，则称为基于函数的视图；如果编写的是类，则称之为基于类的视图。views.py就是保存函数或者类的视图文件。当然也可以用其他的文件名称，只不过在引入响应函数或者类时，要注意名称的正确性，views.py是我们习惯使用的文件名称。

  

除了上述文件，在执行startapp命令后还会自动添加sqlite3数据库，在django中是默认使用这个(如需配置其配置路径为./my_site/settings.py)。通常我们会自己创建static文件夹存放样式、template文件夹存放html静态文件、一起创建其他自己需要的文件夹。使用pycharm创建时会自动生成

## 配置网站管理系统

### 步骤

管理入口：127.0.0.1:8000/admin/

#### 创建超级用户

注：在创建超级用户之前必须得先进行数据迁移，才会产生数据库和管理员表等，不然会报错

```bash
> python manage.py createsuperuser
```

然后会让你输入用户名和密码

#### 配置应用

在应用admin.py中引入自生的models模块（或里面的模型类）

编辑admin.py：admin.site.register(models.***)

进入管理页面即可看到我们应用的数据管理

## 基本功能实现过程

### 模型建立和数据库生成



### 文件上传

#### 上传方式

本次上传文件方式：通过form表单提交到后台

#### 前端

**表单部分**

```html
<!--templates/index.html-->
<div style="position:relative; top:10px; padding-left: 220px">
    
<!--文件上传表单-->
          <form enctype="multipart/form-data" action="/upLoadFile/" method="post">
            <input type="file" name="myfile" />
            <input type="submit" class="btn btn-sm btn-default" value="upload"/>
          </form>
      </div>


```

**文件展示部分**

```html
<!--文件信息展示-->
          <div class="table-responsive">
            <table width="65%" border="0" cellspacing="1" cellpadding="4" class="tabtop13" align="center" rules="rows">
              <thead>
                <tr>
                    <th>文件名</th>
                    <th>大小</th>
                    <th>文件类型</th>
                    <th>上传时间</th>
                    <th>操作</th>
                    <th>操作</th
                </tr>
              </thead>
              <tbody>

                {% for file in files %}
                <tr>
                  <td>{{ file.file_name }}</td>
                  <td>{{ file.file_size }}</td>
                  <td>{{ file.file_type }}</td>
                  <td>{{ file.upTime }}
                  <td>
                    <a href="/file_delete/?name={{file.file_name}}">删除</a>
                  </td>
                  <td>
                    <a href="/download/?name={{file.file_name}}">
                        <i class="fa fa-download" data-toggle=tooltip">下载</i>
                    </a>
                  </td>

                </tr>
                {% endfor %}

              </tbody>
            </table>
          </div>
```

**点击操作后浏览器消息弹窗**

```html
% if messages %}
    <script>
        {% for msg in messages %}
            alert('{{ msg.message }}');
        {% endfor %}
    </script>
{% endif %}
```



#### Django后端

```python
#views.py
# 上传文件
def upLoadFile(request):
    if request.method == "POST":    # 请求方法为POST时，进行处理
        myFile =request.FILES.get("myfile", None)    # 获取上传的文件，如果没有文件，则默认为None
        if not myFile:
            messages.error(request, "no files for upload!")
            return HttpResponseRedirect('/index/')
        
        # 获取表单元素
        useremail = request.COOKIES.get('email', '')#通过cookie获取用户Email
        uploadid = models.UserInfo.objects.get(email=useremail) #从数据库中选出当前用户已上传的文件
        fname = myFile.name
        fsize = myFile.size
        ftype = myFile.content_type
        path = os.path.join("C:\\Upload", myFile.name)
        Time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())

        # 写入数据库
        obj = models.FileInfo(uploader_id=uploadid, file_name=fname, file_type=ftype,file_size=fsize, file_path=path, upTime=Time, key=key, iv=iv)
        obj.save()
        destination = open(os.path.join("C:Upload", myFile.name), 'wb+')    # 打开特定的文件进行二进制的写操作
        for chunk in myFile.chunks():      # 分块写入文件
            destination.write(chunk)
        destination.close()
        messages.success(request, "download over!")
        return HttpResponseRedirect('/index/')

```



### 文件下载

前端在文件上传部分已展示

#### 后端

```python
#views.py
def download(request):
    name = request.GET.get('name')
    def file_iterator(file_name, chunk_size=512):
        with open(file_name) as f:
            while True:
                c = f.read(chunk_size)
                if c:
                    yield c
                else:
                    break
    the_file_name = os.path.join("C:\\Upload", name)
    response = StreamingHttpResponse(file_iterator(the_file_name))
    response['Content-Type'] = 'application/octet-stream'
    response['Content-Disposition'] = 'attachment;filename="{0}"'.format(the_file_name)

    return response
```



### 文件删除

```python
def file_delete(request):
    # 获取要删除的文件名
    name = request.GET.get('name')
    # 获取文件
    file = models.FileInfo.objects.filter(file_name=name)
    # 调用.delete()方法删除数据库文件数据
    file.delete()
    # 删除文件
    my_file = os.path.join("C:\\Users\\sakura\\Desktop\\server", name)
    if os.path.exists(my_file):
        os.remove(my_file)
    # 完成删除后返回主页
    else:
        print('no such file:%s' % my_file)
    
    return HttpResponseRedirect('/index/')

```

