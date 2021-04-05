# AutoTaskManagement
基于 Django_crontab、Xadmin 做一套定时任务管理系统
本项目介绍一个定时任务系统 Jcrontab，它用 Python3研发，并用到 Django_crontab 和 Xadmin 等模块。

我们知道在 Linux 环境下，crontab 是一个周期性的任务服务，我们可以根据它的规则来设定系统性的周期性任务。使用 crontab 时，想必你会遇到一个痛点，就是 crontab 需要通过命令行方式进行管理，比较烦琐，并且没有界面化的交互，通常作单机上的定时任务。

而 Jcrontab 是基于 Python3.6 和 Django1.8 版本开发实现的一套前后台界面化的定时任务管理系统，它可以打通企业间的消息服务接口来发送消息。
工程介绍


这个工程一共分为 3 个子系统，第 1 个子系统为前端任务系统，它主要记录单个任务的执行情况，如执行的时间、执行任务的结果，等等。它同时也可以控制单个任务的一些属性，使我可以对单个任务进行启动、停止或者修改任务周期。

第 2 个组成部分是后台管理系统，后台管理系统主要是基于 Xadmin 框架搭建的。它主要用于编辑、添加或删除任务。

第 3 个组成部分就是脚本录入系统，它是基于 Python 开发的一套脚本，主要是为了方便运维人员在控制台能够快速的通过脚本方式录入任务。这是一个python的脚本(insert_work.py)，用于运维人员方便的在终端新建任务。


工程演示


接下来我们来演示一下这套系统。

首先进入控制台，然后到代码目录下启动工程，并且让它监听 8000 端口，同时打开浏览器，在浏览器的 URL 地址栏里面输入 IP + 端口号（8000），访问路径为 Xadmin。这样就可以进入后台的登录窗口页面。

登录后，我们会看到 Jcrontab 的后台内容，点击任务表，会看到记录所有任务内容，我们看到这里已经有一个任务在里面，如果需要新建任务的话，你可以点击新建任务。

我在原有任务的基础上来进行演示。在这个任务里我设置了它的启动时间、截止时间和工作周期。如果说我需要临时改变周期的话，可以建一个催单工作周期，同时有工作责任人、工作内容等。

我们看到这里有一栏是企业微通知 URL，它作为一个通知的接口进行调用，也就是说当我们执行完这个任务，程序会调用 URL 去发送消息。

接下来的是任务当前的状态、是否开启催单这两个选项。另外，在任务或命令这里会看到一个执行命令，我这里让这个任务执行的是一个 echo 命令，并且在终端把这个内容输出。

最后一项是工作模式，这里一共有三种模式，如果你只想让它发送信息，可以只选信息模式。如果既要发送信息又要同时执行命令，可以选择信息和命令模式。我这里选择的是信息和命令模式。

点击保存，这样我们就完成了一个定时任务的创建。

接下来看单个任务的前台页面。我们访问的地址同样是 IP + 端口，后面加 taskID，那么 taskID ID的数值为多少呢？我们可以看一下，这里的 ID 是 1，所以我在 URL 地址栏里面直接写的是 task ID 1，然后回车，就可以进入前台界面。

前台界面会展示之前执行这个任务的一些历史记录，同时在控制台可以做相应的管理。

这里我点恢复， 点击之后当前页面的一个状态就会由结单状态变成运行状态。与此同时在前台界面，我们看到当前任务的执行周期是每两分钟执行一次，我们可以等两分钟左右的时间来确认这个任务是否有执行。

刚刚我们讲到了 ，我的任务是信息加命令行模式。因此它的信息会发送到我的企业微信接口。那么企业微信接口的地址是怎么创建的？

首先我们在企业微信的工具里面创建一个群，在这个群里添加一个聊天机器人，机器人里有一个 WebHook 的地址，这个地址可以作为 URL 请求调用的地址，应用程序调用它我们就可以在这个群收到发送的信息了。

可以看到这就是我所发送的消息内容，包含通知需求内容和时间，我们也会看到执行的命令是什么。要通过控制台控制这个任务，可以直接点链接，也通过后台链接进入管理后台进行管理。

另外一块就是执行命令的结果。执行命令的结果会以信息的方式输出到群消息里面，同时还配有一张可爱的图片。这说明我的周期性任务已经执行了，并且已经发送消息，我们可以在消息里看到执行命令的结果。

工程部署、安装及启用


接下来，我给你介绍 Jcrontab 工程如何部署、安装及启用。

首先，我们来看如何安装基础环境，课时开始讲到技术栈的时候有几个关键点，首先我们需要安装 Python 3.6，同时需要通过 Python3.6 安装它相关的模块。我们知道 Django 是 Python 开发的一个框架，而 Django_crontab 是属于 Django 的一个模块，在工程的 requirement.txt 中说明的所有的模块都需要我们去进行安装。

另外就是需要安装基础服务，这里用到的一个基础服务是 MySQL，所以你需要安装 MySQL 5.7 版本。


部署方式
安装基础服务
1、基础程序 Mysql5.7 Python3.6
nginx-1.16.1

2、安装模块
Django1.8
工程所需必要安装模块
pip -r install ./requirements.txt

初始化Mysql及配置
1、my.cnf设置数据库字符集 character_set_server=utf8

2、mysql数据库及新建用户：
create database jcrontab;
CREATE USER jeson@'%' IDENTIFIED BY 'jesonc.com';
grant all privileges on jcrontab.* to 'jeson'@'%' with grant option;

3、修改djang配置settings.py中数据库配置信息 DATABASES = {
'default': {
'ENGINE': 'django.db.backends.mysql', # 数据库引擎
'NAME': 'jcrontab', # 数据库名，先前创建的
'USER': 'jeson', # 用户名，可以自己创建用户
'PASSWORD': 'jesonc.com', # 密码
'HOST': 'www.iaskjob.com', # mysql服务所在的主机ip
'PORT': '3306', # mysql服务端口
}
}

4、初始化数据库模型 1）注释settings.py下的如下配置 CRONJOBS = todo() if CRONJOBS is None: CRONJOBS = [] 2）初始化数据库模型 python3.6 manage.py makemigrations
python3.6 manage.py migrate 3）打开（1）之前的注释

创建xadmin后台用户密码
python3.6 manage.py createsuperuser

Username (leave blank to use 'jeson'): jeson Email address: jeson@imoocc.com Password: jesonc.com Password (again): jesonc.com Superuser created successfully.

启动工程
python3.6 manage.py runserver
