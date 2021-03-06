🔸 简介
================================================================================


    用 djangocms + uwsgi + nginx 来搭建web 服务器.
    然后用 bootstarp 弄网站排版.
    最后用 lsync 同步本地 文本 到服务器,
    每篇博客都是一个 txt 文件连接. 



🔸 文件夹架构
--------------------------------------------------------------------------------  

    关键文件位置.供参考

    /home/djcmsenv
    /home/mycms

    /home/mycms/test.py
    /home/mycms/uwsgi.ini
    /home/mycms/uwsgi_params
    /home/mycms/mysite_nginx.conf
    /home/mycms/manage.py
    /home/mycms/mycms/settings.py


🔸 DjangoCMS 技巧.
--------------------------------------------------------------------------------  

    ❗️❗️ content 模式可以直接双击进行简单编辑
    ❗️❗️ 空格        快速切换 structure 和 content 模式
    ❗️❗️ shift+空格  快速定位



🔸 DjangoCMS 迁移.
--------------------------------------------------------------------------------  

    如果要把项目 从一台服务器迁移到另一台服务器. 要怎么办呢.
    用 scp 命令就可以了.

    scp -P 2222 -r /home/mycms/media root@35.194.128.92:/home/mycms

    ⦿ 虚拟环境. 必须重新建立. 各种依赖要重新安装 不能迁移.

    ⦿ 重点文件.
        • 几乎所有的数据 都在 peojiect.db 里面!!!!
        • 还有一个就是 media 文件夹了. 图标什么都在这里..



🔸 千万 经常备份项目. 不然有你哭的
--------------------------------------------------------------------------------  




⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵   Nginx + uwsgi + djangocms  🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️

Nginx + uwsgi + djangocms
================================================================================


🔸 介绍
--------------------------------------------------------------------------------  

    nginx 是一个 http 服务器，与 apache、lighttpd、Microsoft IIS 等属于同类产品;
    nginx 面向的是 html

    uwsgi 实际上也是一个 http 服务器，只不过它只面向 python 网络应用程序。

    网站其实不一定要用纯html写的. 用python也可以. 比如用 diango 也能搭建网站出来.
    但是 nginx 只支持 html; 
    如果你用 python (django) 搭建的网站那么就必须用uwsgi 这个 http 服务器,
    不过 Nginx 毕竟是最流行的web服务器. 因为功能是强大.
    所以就有了 Nginx + uwsgi + python(django) 这方案.

    Nginx 通过 socket 连接 uwsgi.

    ⦿ uwsgi 命令              
        启动uwsgi：   uwsgi --ini uwsgi.ini
        停止uwsgi：   uwsgi --stop uwsgi.pid
        重新加载配置：uwsgi --reload uwsgi.pid


    ❗️❗️ the web client <-> nginx <-> the socket <-> uwsgi <-> Django ❗️❗️
    ❗️❗️ the web client <-> nginx <-> the socket <-> uwsgi <-> Django ❗️❗️




🔸 Nginx + uwsgi + djangocms 原理
--------------------------------------------------------------------------------  

    ❗️the web client <-> the web server(nginx) <-> the socket <-> uwsgi <-> Django❗️
    ❗️the web client <-> the web server(nginx) <-> the socket <-> uwsgi <-> Django❗️
    ❗️the web client <-> the web server(nginx) <-> the socket <-> uwsgi <-> Django❗️



🔸 准备
--------------------------------------------------------------------------------  

    ⦿ 域名解析  
        www.0214.help ➜ 104.224.139.45

    ⦿ 临时关闭防火墙     
        service iptables stop    systemctl stop firewalld

        ❗️ 如果是 gce 这种有外部防火墙的. 必须设置外部防火墙 ❗️
        ❗️ 如果是 gce 这种有外部防火墙的. 必须设置外部防火墙 ❗️
        ❗️ 如果是 gce 这种有外部防火墙的. 必须设置外部防火墙 ❗️


    ⦿ djangocms 要求
        django CMS requires Django 1.8, 1.9 or 1.10 and Python 2.7, 3.3 or 3.4.

    ⦿ djangocms-installer
        djangocms 搭建脚本. 安装这个会自动安装 各种依赖! 强烈建议在虚拟环境中使用

    ⦿ 虚拟环境跨平台.
        ❗️❗️❗️虚拟环境不能跨平台使用! mac 的虚拟环境 不能同步到 centos 上用的❗️❗️❗️
        ❗️❗️❗️虚拟环境不能跨平台使用! mac 的虚拟环境 不能同步到 centos 上用的❗️❗️❗️



🔸 LNMP 新建 nginx 虚拟主机
--------------------------------------------------------------------------------  

    lnmp vhost add
    ================================================
    Virtualhost infomation:
    Your domain: www.0214.live
    Home Directory: /home/wwwroot/www.0214.live
    Rewrite: none
    Enable log: no
    Create database: no
    Create ftp account: no
    ================================================

    ⦿ 重启 Nginx   service nginx restart 
        现在 浏览器输入 www.0214.live 是没页面的.
        因为 /home/wwwroot/www.0214.live 目录下没有 index.html.


🔸 Django 开发服务器测试 ✔︎
--------------------------------------------------------------------------------  

    ❗️❗️ 项目必须建在 home 目录下. 具体原因未知,反正建在root目录下有问题.❗️❗️
    ❗️❗️ 项目必须建在 home 目录下. 具体原因未知,反正建在root目录下有问题.❗️❗️
    ❗️❗️ 项目必须建在 home 目录下. 具体原因未知,反正建在root目录下有问题.❗️❗️

    原因也许是uWSGI默认在www-data用户组！


    ⦿ cd /home

    ⦿ 虚拟环境建立
        • wget 安装   yum install wget
        • pip  安装   wget https://bootstrap.pypa.io/get-pip.py && python get-pip.py
        • 虚拟环境安装  pip install virtualenv
        • 虚拟环境建立  virtualenv djcmsenv 
        • 进入虚拟环境  source /home/djcmsenv/bin/activate

    ⦿ 新建项目
        pip install djangocms-installer && djangocms mycms

        👹 GCE 天坑一个. 如果你用最低配 这里会出错!!!!! 各种报错.
        1共享vcpu + 内存1.7 的那个就可以 坑爹...


    ⦿ 创建项目初始数据表
        python /home/mycms/manage.py migrate

    ⦿ 开启项目外网权限
        vi /home/mycms/mycms/settings.py
        ALLOWED_HOSTS = [] 改成:
        ALLOWED_HOSTS = ['104.224.139.45', 'www.0214.help'] 
        ALLOWED_HOSTS = ['35.194.128.92', 'www.0214.live'] 
        
        这里填的是服务器的公网IP. 和域名

    ⦿ 启动开发服务器   
        python  /home/mycms/manage.py runserver 0.0.0.0:888

    ⦿ 浏览器测试
        104.224.139.45:888 ➜ 出现 cms 登录界面
        35.194.128.92:888


🔸 uwsgi web服务器测试 ✔︎
--------------------------------------------------------------------------------  

    ⦿ 安装 uwsgi  
        pip install uwsgi

    ⦿ 新建 python 测试文件
        目的是为了 测试 uwsgi  能否正常工作

        vi /home/mycms/test.py

def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return ["Hello World"] # python2
    #return [b"Hello World"] # python3


        ❗️❗️❗️ python 必须注意缩进,def 行首; 缩进空格数量统一!❗️❗️❗️
        ❗️❗️❗️ python 必须注意缩进,def 行首; 缩进空格数量统一!❗️❗️❗️


    ⦿ 测试文件执行权限赋予
        chmod 755 /home/mycms/test.py

    ⦿ uwsgi 运行 test.py 测试文件
        uwsgi --http :999 --wsgi-file /home/mycms/test.py

    ⦿ 浏览器测试
        104.224.139.45:999 ➜ 出现 helloworld
        这里如果出现问题. 必须看 test.py的缩进... 


🔸 uwsgi 运行 python项目  ✔︎
--------------------------------------------------------------------------------  

    cd /home/mycms
    下面命令进行必须在 /home/mycms  项目下运行.

    uwsgi --http :777 --chdir /home/mycms --home=/home/djcmsenv --module mycms.wsgi
    
        --chdir 指定项目路径.
        --home  指定虚拟环境路径
        --module 项目名.wsgi


    ⦿ 浏览器测试
        104.224.139.45:777 ➜ 出现cms 登录界面




🔸 nginx ➜ uwsgi 配置
--------------------------------------------------------------------------------  

    ⦿ 新建 uwsgi_params文件  
        该文件必须在项目目录下.该文件不需要任何修改.直接复制就行.
        vi /home/mycms/uwsgi_params

uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name; 



    ⦿ 新建 nginx 虚拟主机配置文件 mysite_nginx.conf 
        一般也在项目目录下, 在 nginx.conf 中把这文件 include 进去就可以了

        vi /home/mycms/mysite_nginx.conf

upstream django { server 127.0.0.1:333; # 填 uwsgi 端口  }
server {
    listen      222;                    # 填 nginx 监听端口
    server_name www.0214.help;          # 填 nigix 监听域名
    charset    utf-8;
    client_max_body_size 75M;

    location /media  {   alias /home/mycms/media;  # 填项目media  路径 }
    location /static {   alias /home/mycms/static; # 填项目static 路径 }
    location /       {
        uwsgi_pass  django;
        include /home/mycms/uwsgi_params;       # 填项目 uwsgi_params 路径
    }
}


    👁‍🗨  解析
        uwsgi_pass  django; 
        upstream django { server 127.0.0.1:333;  }

    ❗️❗️ uwsgi_pass 让Nginx把到的所有请求都转发到”127.0.0.1:333″也就是 uWSGI服务器上。
    ❗️❗️ uwsgi_pass 让Nginx把到的所有请求都转发到”127.0.0.1:333″也就是 uWSGI服务器上。



    ⦿ 修改 nginx 默认配置
        把上面的 mysite_nginx.conf include 到默认的 nginx.conf 中才会生效!!!        

        vi /usr/local/nginx/conf/nginx.conf

        include vhost/*.conf; 行下面添加
        include /home/mycms/mysite_nginx.conf;

            👁‍🗨 如果你配置过其他虚拟主机. 那么最好注释掉其他所有 *.conf 
            暂时只留 include mysite_nginx.conf.
            免得 其他的配置文件 对 mysite_nginx.conf 产生影响.


            👁‍🗨  Mac os nginx 
                ⦿ nginx 安装路径
                    type nginx 来查找. nginx is /usr/local/bin/nginx

                ⦿ Mac brew nginx 安装路径
                    brew 安装的软件都在 /usr/local 文件夹下.
                    把所有软件的 bin 都放在 /usr/local/bin 下
                    把所有软件到 配置文件 都放 /usr/local/etc下.
                    所有 Mac 的nginx 配置文件路径是: 
                    vi /usr/local/etc/nginx/nginx.conf

            👁‍🗨  虚拟主机配置文件 优先级
                ❗️❗️ 默认配置文件 优先级 小于 虚拟主机配置文件优先级 ❗️❗️
                ❗️❗️ 默认配置文件 优先级 小于 虚拟主机配置文件优先级 ❗️❗️

                默认配置文件. 最好一般都会 include 所有虚拟主机配置文件的配置.
                默认配置文件的某些设置是会被 虚拟主机的配置文件 覆盖掉的!!
                所以尽量 创建虚拟主机. 尽量在虚拟主机的配置文件中进行修改.



    ⦿ 项目静态文件(不可省略)
        django项目 静态文件路径本来是在/home/mycms/mycms/static 的. 
        不知为什么 要改成 /home/mycms/static

        vi /home/mycms/mycms/settings.py
            尾部添加行
            STATIC_ROOT = os.path.join(BASE_DIR, "static/")

        复制 /home/mycms/mycms/static 到 /home/mycms/static
            python /home/mycms/manage.py collectstatic
            输入 yes

        • 关闭 apache service httpd stop
        • 重启 nginx  sudo /etc/init.d/nginx restart

        • media 测试
            为什么不是 static 文件夹... 不知道..
            cd /home/mycms/media && wget http://picture.baidu.com/media.png

        • 浏览器测试  http://www.0214.help:222/media/media.png
        👹 项目建在 root 目录下就不行. 这里会出现 403. 在 home 目录下就正常!!!
        👹 项目建在 root 目录下就不行. 这里会出现 403. 在 home 目录下就正常!!!




🔸 nginx ➜ uwsgi(http) ➜ test.py
    ⦿ 进入虚拟环境
        source /home/djcmsenv/bin/activate && cd /home/mycms

    ⦿ 启动uwsgi
        uwsgi --socket :333 --wsgi-file /home/mycms/test.py

            注意 这里的 333 不是随便填的
            要和 mysite_nginx.conf 
            upstream django { server 127.0.0.1:333;  } 一样.

    ⦿ 浏览器测试    www.0214.help:222    出现 hello world



🔸 nginx ➜ uwsgi(http) ➜ jangocms 

    ⦿ 运行uwsgi
        uwsgi --socket :333 --chdir /home/mycms --home=/home/djcmsenv --module mycms.wsgi

        👁‍🗨 上面的 test.py 不需要虚拟环境. 是因为 test.py 只是一个 py文件.
        这里是一个 jangocms 项目. 项目需要很多依赖! 这些依赖都是安装在 虚拟环境里的.
        所以这里必须加虚拟环境的路径. 不然肯定抱错.

    ⦿ 浏览器测试  www.0214.help:222      出现 cms 的登录页面.



🔸 nginx ➜ uwsgi(socks) ➜ test.py

    到目前为止,我们使用了TCP的socket端口, 
    但事实上, 使用Unix sockets比ports更好.但是需要做一点小小的改动.

    ⦿ 配置 nginx 文件
        vi /home/mycms/mysite_nginx.conf

            upstream django { server 127.0.0.1:333;  }
            改成 
            upstream django { server unix:///home/mycms/mycms.sock; }

            👁‍🗨 这个mycms.sock 会自动生成的. 不用关心.
            
    ⦿ 重启nginx    sudo /etc/init.d/nginx restart


    ⦿ 运行 uwsgi
        uwsgi --socket mycms.sock --wsgi-file test.py --chmod-socket=666

    ⦿ 浏览器测试 http://www.0214.help:222/ 就能看到 Hello World


🔸 nginx ➜ uwsgi(socks) ➜ djangocms 
    ⦿ 运行uwsgi
        uwsgi --home /home/djcmsenv --socket mycms.sock --module mycms.wsgi --chmod-socket=666

        ❗️❗️  uwsgi 命令必须加 虚拟路径!! ❗️❗️
        ❗️❗️  uwsgi 命令必须加 虚拟路径!! ❗️❗️
        ❗️❗️  uwsgi 命令必须加 虚拟路径!! ❗️❗️

        ❗️ 这个命令 必须在 /home/mycms 目录下执行 ....
        ❗️ 这个命令 必须在 /home/mycms 目录下执行 ....


    ⦿ 浏览器测试  http://www.0214.help:222/   出现 cms 的登录页面了.



🔸 用配置文件运行uWSGI.
    uwsgi 很多配置的. 全部写在命令行中太长了.也记不住. 所以要配置文件.
    uwsgi8800.ini  文件名随便取. 一般是 uwsgi+端口号

    ⦿ 新建 uwsgi 配置文件
        vi /home/mycms/uwsgi.ini

[uwsgi]
home = /home/djcmsenv
chdir = /home/mycms
module = mycms.wsgi
master = true
processes = 10
socket = /home/mycms/mycms.sock
chmod-socket    = 666
vacuum = true

    ⦿ 启动 uwsgi
        uwsgi --ini /home/mycms/uwsgi.ini 

    ⦿ 浏览器测试 http://www.0214.help:222/   出现 cms 的登录页面了.


🔸 虚拟环境.

    如果你的 dj项目 使用虚拟环境.那么你要知道
    ❗️如果要使用 uwsgi 那么不管是不是在虚拟环境中, 都必须指定虚拟环境路径❗️
    ❗️如果要使用 uwsgi 那么不管是不是在虚拟环境中, 都必须指定虚拟环境路径❗️
    ❗️如果要使用 uwsgi 那么不管是不是在虚拟环境中, 都必须指定虚拟环境路径❗️

    ⦿ 系统环境安装 uwsgi
        pip install uwsgi
            必须在 虚拟环境中 和 真实环境中同时装一遍 uwsgi 
            之前我们是在虚拟环境中装的. 现在要在系统环境中再安装一遍.

    ⦿ 运行uwsgi
        uwsgi --ini /home/mycms/uwsgi.ini --chmod-socket=666
            由于我们在配置文件中指定了 虚拟环境的路径. 
            所以命令行中就可以不用设置虚拟环境路径了.
            所以这个命令不在虚拟环境中也能运行.


🔸 排错

    重启服务器 
    然后用 uwsgi --ini /home/mycms/uwsgi.ini 
    浏览器测试失败.

    重启电脑后发现 nginx 没有启动
        可能是没有设置开机启动
        也有可能设置了开机启动.但是由于和apache也是开机启动的. 
        默认nginx 和 apache 都是用 80端口的. 
        系统启动时候 先启动了 apache. 使用了 80端口.
        然后再启动nginx 也要使用 80端口. 
        两者冲突导致nginx开机启动失败

    chkconfig --list   查看开启启动状态列表 没发现 apache啊.
    估计 不是用 chkconfing 方式自动启动的.




🔸 开机启动设置
    ⦿ 自动启动配置文件
        chmod 755 /etc/rc.d/rc.local
    
        vim /etc/rc.local
        在exit 0这一行之前加上下面这3句话

        service httpd stop
        service nginx start
        uwsgi --ini /home/mycms/uwsgi.ini

    ⦿ 浏览器测试 http://www.0214.help:222/   出现 cms 的登录页面了.


    ⦿ 端口更改.
        网页么端口不是80 就是 443 .其他端口只是避免冲突测试用的
        vi /home/mycms/mysite_nginx.conf 把222 改成80 就是不行.
        可能是80被占用了..  但是不是被nginx 占用了.
        lsof -i:端口号 安装: yum install lsof
        如lsof -i:80 
        nginx     15891 root    6u  IPv4 147886      0t0  TCP *:http (LISTEN)
        nginx     15892  www    6u  IPv4 147886      0t0  TCP *:http (LISTEN)
        两个http.... 

        ✘✘∙GCE ~ ➜     cd /usr/local/nginx/conf/vhost/
        居然看到个 www.0214.live的配置文件 里面默认用的80端口..
        原来这就是我们用lnmp 建立的虚拟主机...
        其实没必要. 我们已经在mycms项目里面建了一个 .conf 所以就冲突了..
        删除...

        rm /usr/local/nginx/conf/vhost/www.0214.live.conf
        或者 lnmp vhost del...





⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 mycms 配置 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️


🔸 后台帐号创建
================================================================================

    cd /home/mycms && source /home/djcmsenv/bin/activate

        python manage.py createsuperuser

    然后网页登录 http://www.0214.help:222/

    自动弹出的框. 点击 new page  ➜ next ➜ title: home ➜ create
    现在你的网页 就有一个 About 链接了.. 
    实在太简陋了...  现在是涉及 html 前端的知识.. 
    简单点下个模板稍微修改下... 
    博客主要的是内容.. 外观么... 有精力可以搞搞..



🔸 bootstarp 插件安装.
================================================================================

    ⦿ 这个插件可以让你快速建立网页布局.

    cd /home/mycms && source /home/djcmsenv/bin/activate


    ⦿ 安装 bootstrap 第三方模块.
    pip install aldryn-bootstrap3

    ⦿ 配置
    vi /home/mycms/mycms/settings.py 
    添加 'aldryn_bootstrap3', 到 INSTALLED_APPS
    注意逗号.

    ⦿ 生成bootstrap插件所需的数据库表
    python /home/mycms/manage.py migrate

    ⦿ 重启服务器...

    ⦿ 测试
        就多了个..  bootstarp 插件可以用



🔸 首页修改.
================================================================================

⦿ 模板下载.

    • 模板地址  https://startbootstrap.com/template-overviews/modern-business/

    • 下载模板+解压+重命名
        cd / && wget https://github.com/BlackrockDigital/startbootstrap-modern-business/archive/gh-pages.zip && unzip gh-pages && mv startbootstrap-modern-business-gh-pages modern

⦿ 文件移动 css、font-awesome、fonts、js
    mv modern/css /home/mycms/mycms/static && mv modern/font-awesome /home/mycms/mycms/static && mv modern/fonts /home/mycms/mycms/static &&mv modern/js /home/mycms/mycms/static

⦿ 删除下载的模板
    rm -rf /modern


⦿ 删除djangocms 里无用模板:base+left+right
    留 fullwidth.html 因为这个是djangocms的默认模板,暂时不能删除

    cd /home/mycms/mycms/templates && rm -f /home/mycms/mycms/templates/base.html /home/mycms/mycms/templates/sidebar_left.html /home/mycms/mycms/templates/sidebar_right.html



⦿ 新建基础模板   
    vi /home/mycms/mycms/templates/base.html


{% load cms_tags menu_tags sekizai_tags staticfiles i18n %}
<!--为了使用模板的基本功能. 首先需要在最上方添加这行-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">

    <!-- 模板里调用静态文件 用下面的格式.-->
    <link href="{% static 'css/bootstrap.min.css' %}" rel="stylesheet">
    <link href="{% static 'css/modern-business.css' %}" rel="stylesheet">
    <link href="{% static 'font-awesome/css/font-awesome.min.css' %}" rel="stylesheet" type="text/css">

    <!--引入 djangocms 以及各种插件 所依赖的css文件-->
    {% render_block "css" %}

    <!--模板的标题-->
    <title>{% page_attribute 'page_title' %} - Xu.Jian </title>
</head>

<body>
    <!--cms 隐藏工具条-->
    {% cms_toolbar %}


    <!-- 网页导航条 -->
    <nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
        <div class="container">

            <!-- Brand and toggle get grouped for better mobile display -->
            <div class="navbar-header">
                <!--home 按钮-->
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="/">Home</a>
            </div>

            <!--导航条中的 home之外的按钮.-->
            <!-- Collect the nav links, forms, and other content for toggling -->
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav navbar-right">
                {% show_menu 0 100 100 100 %}
                </ul>
            </div>
            <!-- /.navbar-collapse -->
        </div>
        <!-- /.container -->
    </nav>


    <!--下面是网页的内容区域.  block xxx 就代表模块,  可以在模板里面用别的模板-->
    {% block content %}{% endblock %}

 
    <!--可以在 结构中 添加一个cate 区域.-->
    {% placeholder Cate %}
    {% placeholder Tage %}
    {% placeholder File %}



    <!-- Footer -->
    <footer>
        {% static_placeholder "Footer"%}
    </footer>
        
    <!-- jQuery -->
    <script src="{% static 'js/jquery.js' %}"></script>

    <!-- Bootstrap Core JavaScript -->
    <script src="{% static 'js/bootstrap.min.js' %}"></script>
        {% render_block "js" %}

</body>

</html>




⦿ 模块改进..
    其实 静态文件 完全可以用 cdn 提供的 比你自己的快多了.
    注意版本!!! bootstrap 和 jquery 不是随便选的!! 有兼容性问题的.

    bootstrap.min.js 3.3.7
    <script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

    jquery.js 版本要求 1.11.1
    <script src="https://cdn.bootcss.com/jquery/1.11.1/jquery.js"></script>

    bootstrap.min.css 3.3.7 
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    font-awesome.min.css
    <link href="https://cdn.bootcss.com/font-awesome/4.7.0/css/font-awesome.css" rel="stylesheet">






⦿ 静态文件处理
    由于之前 静态文件路径改过了...
    所以要再一次运行复制 /home/mycms/mycms/static 新增文件到 /home/mycms/static

    source /home/djcmsenv/bin/activate && python /home/mycms/manage.py collectstatic

    

⦿ 测试网页. 
    http://www.0214.help:222/en/
    现在就有一个 漂亮的顶部导航条了: 左边是Home 右边是About
    其实模板就是一些静态文件 + 一个 bootstrap 的首页而已
    优点就是 可以适应所有设备. 自动改变网页宽度. 排版好看..

    接下来的网页就看你自己的了.



⦿ 网站图标修改

    图标是在html网页里面设置的.
    然后 上传图标 设置路径.
    favicon.ico 


    有个网站. 方便设置所有平台全套的图标.

    上传一张png 图片. 
    然后下载一个文件夹..
    然后把 文件夹 放到服务器的 /home/mycms/static/favicon 文件夹...

    http://www.0214.live/static/favicons/favicon.ico
    然后测试 是不是正常.

    然后 就是html代码了额
    放到.../home/mycms/mycms/templates/base.html 下的 head里面..
    最后重启nginx 就可以了. service nginx restart



    <link rel="apple-touch-icon" sizes="180x180" href="/static/favicons/apple-touch-icon.png">
    <link rel="icon" type="image/png" sizes="32x32" href="/static/favicons/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="/static/favicons/favicon-16x16.png">
    <link rel="manifest" href="/static/favicons/manifest.json">
    <link rel="mask-icon" href="/static/favicons/safari-pinned-tab.svg" color="#5bbad5">
    <meta name="theme-color" content="#ffffff">




🔸 HTTPS
================================================================================

    首先去阿里云 申请免费的 ssl证书. www.0214.live
    然后下载 Nginx 版本的证书
    得到两个文件:  .key  和  .pem
    服务器新建cert 文件夹: mkdir /usr/local/nginx/conf/cert
    然后把两个证书文件 传到 cert 目录下


vi /home/mycms/mysite_nginx.conf  添加 https 模块

server {
    listen 443;
    server_name www.0214.live;

    location /media  {   alias /home/mycms/media;  }
    location /static {   alias /home/mycms/static; }
    location /       {
      uwsgi_pass  django;
       include /home/mycms/uwsgi_params;
      }


    ssl on;
    ssl_certificate /usr/local/nginx/conf/cert/214194220970463.pem;
    ssl_certificate_key /usr/local/nginx/conf/cert/214194220970463.key;
    ssl_session_timeout 5m;
    ssl_protocols SSLv2 SSLv3 TLSv1;
    ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
    ssl_prefer_server_ciphers on;

}

测试: https://www.0214.live  就可以了




⦿ nginx https 跳转 
    最好把 www.0214.live 转到 https 下.

    vi /home/mycms/mysite_nginx.conf 
    在 80 模块下 添加下面行就可以了

    rewrite ^(.*)$  https://$host$1 permanent;  












🔵 备份.
    vps 难免折腾..  
    博客算重要内容了.. 必须备份..
    两种备份方法: git 或者 lsync
    git 只要注册个帐号就能用了.
    lsync 需要你有一个公网IP的电脑.
    git的缺点是 手动备份. 要commint后才会备份.
    lsync 优点是 自动备份. 有任何修改就会自动同步到远端服务器.

🔸 git 备份.
================================================================================

    git 安装      yum install git 
    查看版本      git --version
    初始化仓库    git init
    当前仓库状态  git status

    添加文件（.或者*表示全部添加）   git add .
    提交代码  git commit -m "commit message" 
    添加远程仓库   git remote add origin git@github.com:xxxx.git 
    push本地代码到远程仓库  git push -u origin master 



🔸 GitHub 准备 
================================================================================

    ⦿ github 创建一个空仓库:mycms
        为了避免冲突，先不要勾选 README 和 LICENSE 选项

    ⦿ 获取仓库地址. 
        项目右上角 clone or download > 获取 https 地址.
        https://github.com/Xu-Jian/mycms.git



🔸 VPS 准备

    ⦿ 初始化仓库
        cd /home/mycms && git init
        mycms 下多了个 .git文件就说明成功了.
        下面我们要 所有文件 加到git仓库.

    ⦿ git add .
        可以用 .gitignore文件来添加不需要同步的文件

    ⦿ 提交修改(本地):  git commit -m "first commit"
        提交一次. 就可以还原.  不提交的内容是无法还原的.

    ⦿ 配置 github 用户名

        如果不用github的话.就上面几步了. 
        如果要用github的话 就得先配置 github的帐号密码.

        添加 github 用户名  $ git config --global user.name "Xu-Jian"
        添加 github 邮箱    $ git config --global user.email "xujian0219@126.com"



    ⦿ 配置 github 密钥
        需要在vps上单独生成一个密钥(用你的邮箱地址)! 不能用以前的.

        $ ssh-keygen -t rsa -C xujian0219@126.com
        一路回车 就会在 /root/.ssh/ 下面生成两个文件.
        一个 id_rsa 是私钥. 一个 id_rsa.pub 是公钥.

        然后去github 的系统设置 > ssh and gpg keys > new ssh key >
        要你输入 名称 和 key  名称随便. key 就是服务器的公钥里面的内容.

        复制公钥的所有内容. cat /root/.ssh/id_rsa.pub 到github的key 内容框里

    ⦿ 测试密钥设置
        $ ssh -T git@github.com
        如果成功设置 就会有个 
        Are you sure you want to continue connecting (yes/no)? 
        输入yes 就可以了


    ⦿ 查看仓库情况    git remote -v

    ⦿ 远程到github
        git push -u origin master

    ⦿ 查看同步情况
        现在去github 的 mycms项目. 就多出了本地的文件了. 同步成功!!!









⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
aldryn-bootstrap3 插件
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
官方文档:  https://pypi.python.org/pypi/aldryn-bootstrap3/1.2.0
官方文档:  https://pypi.python.org/pypi/aldryn-bootstrap3/1.2.0
官方文档:  https://pypi.python.org/pypi/aldryn-bootstrap3/1.2.0

插件就是一堆 html ... 只是预先写好一些代码而已.
你可以在html中添加 class 来实现插件的功能.. 也能直接用插件...





    🔸 导航栏: 网络安全 + 前端 + 后端 + Misc + 简历




🔸 style    容器. div .  
    容器也是有默认 边距的.
    建议所有元素都放进 div 里面. 也就是 style 里面.

    class name 选择 container.   
    tag 选择  div ..


🔸 Row  行.
    create columns   
    col-xs- offset-  push- pull-
    col-md- offset-  push- pull-
    col-lg- offset-  push- pull-

    row 是创建行的意思.  一行分成12部分.
    create columns   一行分成几列. 1 就是1

    col-xs 是小屏幕   xs- 建议值 4. 
    col-md 是中屏幕
    col-lg 是大屏幕
    这里填12 就是说小手机上为了充分利用屏幕宽度. 不留任何边距
    不然的话 默认每行两边都有一定的默认 屏幕越大. 边距越大..

    当然如果 row 是在 容器内 也就是 style 里面.
    那么容器是有留默认边距的. 里面的row 再怎么利用 也不会超出容器.


🔸 text       文字.  class="page-header" 可以加上一个下划线.

文本内是可以添加图片/图标的. 可以用 代码模式编辑.
<p><cms-plugin alt="Icon - glyphicon-time " id="821" render-plugin="true" title="Icon - glyphicon-time"></cms-plugin> <span> 2017-07-10-19 </span></p>

比如一个时间图标.  加上 span 就可以在同行内添加时间

比如星星图标. 
<cms-plugin alt="Icon - glyphicon-star " id="822" render-plugin="true" title="Icon - glyphicon-star"></cms-plugin>

重复5次就有5个星星了...




⦿ ★★★★★ 居中显示
<p style="text-align: center;"><cms-plugin alt="Icon - glyphicon-star " id="823" render-plugin="true" title="Icon - glyphicon-star"></cms-plugin><cms-plugin alt="Icon - glyphicon-star " id="823" render-plugin="true" title="Icon - glyphicon-star"></cms-plugin><cms-plugin alt="Icon - glyphicon-star " id="823" render-plugin="true" title="Icon - glyphicon-star"></cms-plugin><cms-plugin alt="Icon - glyphicon-star " id="823" render-plugin="true" title="Icon - glyphicon-star"></cms-plugin><cms-plugin alt="Icon - glyphicon-star " id="823" render-plugin="true" title="Icon - glyphicon-star"></cms-plugin></p>



⦿ 时间图标 居中

<p style="text-align: center;"><cms-plugin alt="Icon - glyphicon-time " id="824" render-plugin="true" title="Icon - glyphicon-time"></cms-plugin> 2017-07-10 </p>



⦿ 行内 一左一右


靠右

<p>

<span>sdfr&nbsp;</span>

<span style="text-align: right;">sdfr&nbsp;</span>

</p>


⦿ 内联元素.
html 块元素会重启一行. 内联元素则是一行内从左到右

又有小伙伴问了，设置两个元素一左一右可以实现一行显示吗？当然可以：


作者： 木晨 
链接：http://www.imooc.com/article/2058
来源：慕课网




🔸 bkcokquote 引用.  右边有个引用的标志.
🔸 Panel      面板.  标题 + 内容 + 页脚
🔸 Carousel   图片轮播!!!
🔸 Jumbotron  巨大的显示屏. (背景为灰色的 DIV)
🔸 Lable      标签. 小按钮图标.
🔸 tab        一行内 多个链接/按钮 ?


🔸 well ... 
🔸 file       文件下载   
🔸 accordion  手风琴 点击下方会出现解释的.  不能居中是问题








🔸 Content

column:30、xs:4、sm:3、md:2、lg:2 
        意思是 每行最长30列. 当宽度就自动换行.  
        特小屏的时候. 每列占一行的4  也就是一行可以放 3列.
        小屏幕的时候. 每列占一行的3  也就是一行可以放 4列.
        中屏幕的时候. 每列占一行的2  也就是一行可以放 6列.
        大屏幕的时候. 每列占一行的2  也就是一行可以放 6列.







    🔸 博客页脚
    style 
        row
            columns:1、col-xs:12
                text > Copyright @ 2017 All rights reserved


🔸 分类标签
style: VPS
    row1-1 > columb:1、xs:12
        text > <h3 class="page-header" ><strong>VPS</strong></h3>


    
    🔸 重要博客模块 一排两个. 文字+图标、图标+文字
    row1-1
        cloumn2-1: 6666 > link > row ➜ 一行内的 左半部分.
                cloumn 8888 > panel body > text   ➜ 左边的 左边部分    
                column 4444 > image               ➜ 左边的 右边部分
                    
        column2-2: 6666 > link > row ➜  一行内的右半部分
                column 4444 > image               ➜ 右边的 左边部分
                cloumn 8888 > panel body > text   ➜ 右边的 右边部分 


    🔸 次要博客模块 一排6个.
    row6-1 > column:1、xs:4、sm:3、md:2、lg:2 
        link/Button: button: Block
        link/Button: link >www...
            Panel
                panel body
                    image
                panel footer
                    text
    row6-2 
    row6-3 
    row6-4 
    row6-5 
    row6-6 
    
    






🔵  首页结构.


🔸 轮播图


每个文章由 标题+图片+简介组成
每个文章 都是一个链接.




⦿ 创建大类标签
    就是直接建立..  new page . 显示在顶部工具栏的...
    然后在 大类标签小 手动建立小标签.
    小标签里面 放具体博客txt链接
    博客就是这样了..



下面就该设置 网页链接了. 
文件要有链接.  文件还要和本地保持一致..

要么用xx网盘. 网盘的链接太麻烦了..
要么用自己服务器搭 lsync... 要有个稳定的服务器..
实在不行.. 用github ...经常手动 提交更新就可以保持文件最新...


我们选择... 自己服务器把..
还有就是路径问题...  服务器上的文件不是谁都可以访问的..
简单简单点就 放在/home/mycms/media 文件夹里面..
要么就自己建个FTP服务器 什么的 ....

我们还是用git吧.... git安全.. 本地电脑坏了 还能服务器上找回来.






⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
lsync 设置.
虚拟环境单独安装. mac上安装一遍. vps上再安装一遍.
mycms 文件夹 和 vps 文件夹则由mac 同步到服务器. 



⦿ 配置.
sudo vi /etc/lsyncd-gce.conf


settings {
        logfile = "/tmp/lsyncd-gce.log",
        statusFile = "/tmp/lsyncd-gce.status",

        insist = true,
        nodaemon   = true,

        inotifyMode = "CloseWrite or Modify",
        -- 怎么才算一次修改. 是保存呢,还是任何修改.

        statusInterval = 5,
        --每隔5秒查看一次本地是否有修改.

        maxDelays = 1,
        -- 本地有多少次修改 才会触发同步.

}

sync {
        default.rsyncssh,

        source = "/Users/v/Desktop/gce",

        host="35.194.128.92",

        targetdir = "/root/gce",

        excludeFrom = {
            /Users/v/Desktop/gce/djangocms,
            /Users/v/Desktop/gce/mycms,
        },


        rsync = {
                binary = "/usr/local/bin/rsync",
                archive  = true,
                compress = true,
                whole_file = false

        },

        ssh = {
                port = 22,
                identityFile ="/Users/v/.ssh/id_rsa",
        }
}
 


⦿ ❗️❗️❗️ 运行 
sudo lsyncd /etc/lsyncd-gce.conf














































🔵 项目简介
    djangocms 是基于 django的.  所有的django功能在djangocms里都能用.
    没django基础的 可以边学习djangocms 边了解 django. 
    有django基础的 可以更好的理解 djangocms.
    你在网上看到的教程. 也可以直接在djangocms项目里面运行/测试.

    学习python的朋友或多或少都会想用python来架构一个自己CMS网站。

    Django 也是在框架中中提供了开发网站经常用到的模块，常见的代码都为你写好了，通过减少重复的代码，Django 使你能够专注于 web 应用上有 趣的关键性的东西。为了达到这个目标，Django 提供了通用Web开发模式的高度抽象，提供了频繁进行的编程作业的快速解决方法，以及为“如何解决问题”提供了清晰明了的约定。Django的理念是DRY(Don't Repeat Yourself)来鼓励快速开发！


    让我们一览 Django 全貌
    🔸 urls.py
    网址入口，关联到对应的views.py中的一个函数（或者generic类），访问网址就对应一个函数。

    🔸 views.py
    处理用户发出的请求，从urls.py中对应过来, 通过渲染templates中的网页可以将显示内容，比如登陆后的用户名，用户请求的数据，输出到网页。

    🔸 models.py
    与数据库操作相关，存入或读取数据时用到这个，当然用不到数据库的时候 你可以不使用。

    🔸 forms.py
    表单，用户在浏览器上输入数据提交，对数据的验证工作以及输入框的生成等工作，当然你也可以不使用。

    🔸 templates 文件夹

    🔸 views.py 中的函数渲染templates中的Html模板，得到动态内容的网页，当然可以用缓存来提高速度。

    🔸 admin.py
    后台，可以用很少量的代码就拥有一个强大的后台。

    🔸 settings.py
    Django 的设置，配置文件，比如 DEBUG 的开关，静态文件的位置等。


🔵 参考资源

    👁‍🗨 如果本文理解有困难.建议按顺序阅读下面参考资源. 

    1. ★★★★★   Django Girls 教程:   https://tutorial.djangogirls.org/zh/  
                    ❗️❗️❗️ 非常非常容易理解.结合实际例子. 务必先看这个教程, 里面有不懂的可以结合本文 !!!
                    ❗️❗️❗️ 非常非常容易理解.结合实际例子. 务必先看这个教程, 里面有不懂的可以结合本文 !!!


    2. ★★★★★   简书:开始你的第一个Django应用，第一章  http://www.jianshu.com/p/9d9281e5ebc8           
    3. ★★★★★   简书: 开始你的第一个Django应用(part 2)  http://www.jianshu.com/p/fc049fd5e2a9
    也是一个实例子. 


    django-cms 官方详细说明手册:    http://docs.django-cms.org/en/latest/how_to/install.html

    参考网站:          https://blackrockdigital.github.io/startbootstrap-modern-business/index.html
    参考网站源码下载:  https://startbootstrap.com/template-overviews/modern-business/





🔵 储备知识

    🔸 CMS 简介
        CMS 的意思是 Content Management System 内容管理系统，
        一般拿就可以使用，还可以在原来的基础上再次开发，减少工作量，
        著名 CMS: django-cms、mezzanine、wagtail


    🔸 网页开发: 
        前端: html、css、js  
        后端: php、nodejs、ruby、python(Django)、

        django 就是python的一个著名的 web 开发框架.偏后端.管理网页.学python 就是学 diango.


    🔸 项目 & APP:

        一个网站就是一个项目,一个网站有很多功能(模块)组成. 
        模块就是Django里的APP!! Django里创建APP 就是创建模块
        模块可以重复利用. 甚至可以在别的项目里调用.


    🔸 项目位置
        把任何Python代码放在你的Web服务器的文档根目录里都并不是一个明智的做法，
        项目一般是网站. 网站里的很多文件都是必须让任何人都可以访问的.
        把项目放在服务器根目录.可能导致别人可以访问项目之外的文件.很不安全
        一般建议放在 /home


    🔸 Django项目结构
            新建Django项目后就自动建立了很多文件.
            这些自带文件可以修改,但是千万不要随意的去移动以及重命名.

              my_blog                项目容器, 可以任意修改文件夹名
              ├── manage.py       ➜ 主要脚本，用于和Django交互。
              └── my_blog         ➜ 项目Python包,不可重命名!
                  ├── __init__.py ➜ 空文件，表示当前目录是一个Python包
                  ├── settings.py ➜ Django项目配置文件
                  ├── urls.py     ➜ 管理项目的网址. 
                  └── wsgi.py     ➜ 搭建正式web服务器 需要用到这个文件.



    🔸 admin 基础
        Django 独特之处在于内置了基于 web 的管理程序 -- admin
        Django 自动管理工具是 django.contrib 的一部分。
        你可以在项目的 settings.py 中的 INSTALLED_APPS 看到它：
        django.contrib是一套庞大的功能集，它是Django基本代码的组成部分。


        ⦿ 启用admin
        项目 urls.py 中加入这行就可以了 from django.contrib import admin

        ⦿ 创建 admin 登录用户:
        管理么 肯定需要登录后才能管理的. 登录账户需要先创建
        python manage.py createsuperuser

        ⦿ 访问 admin 
        运行开发服务器 ➜ 浏览器 ➜ 23.105.192.96:8000/admin


    🔸 admin 页面自定义

        我们可以自定义管理页面，来取代默认的页面。比如上面的 "add" 页面。我们想只显示 name 和 email 部分。修改 TestModel/admin.py:

        from django.contrib import admin
        from TestModel.models import Test,Contact,Tag
        
        # Register your models here.
        class ContactAdmin(admin.ModelAdmin):
            fields = ('name', 'email')
        
        admin.site.register(Contact, ContactAdmin)
        admin.site.register([Test, Tag])

        以上代码定义了一个 ContactAdmin 类，用以说明管理页面的显示格式。
        里面的 fields 属性定义了要显示的字段。


    🔸 admin 进阶
        admin 其实很强大  http://python.jobbole.com/84340/
        能处理各种东西. 前提是你会用.. 






    🔸 数据库

        django 的数据库默认用sqlite . 也可以用: mysql、postgresql
        
        使用SQLite，你不需要提前创建任何东西，数据库文件会在必要的时候自动创建。
        使用SQLite之外的数据库:
            请确保你使用Django前已经创建了数据库!!
            然后编辑 settings.py 下的 Engine、Name、USER、Password、Host、Port. 
                Engine字段 参考下面的值; 其他填你自己的数据库信息. 我会给出一个mysql的配置例子.
            
            注: Engine 是数据库驱动. 不同的数据库用不同的字段. 
                django.db.backends.postgresql  # PostgreSQL  
                django.db.backends.mysql       # mysql  
                django.db.backends.sqlite3     # sqlite  
                django.db.backends.oracle      # oracle 

            ⦿ 默认配置(sqlite)

                    DATABASES = {
                        'default': {
                            'ENGINE': 'django.db.backends.sqlite3',
                            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
                        }
                    }

            ⦿ mysql 配置参考

                    DATABASES = {
                        'default': {
                            'ENGINE': 'django.db.backends.mysql',
                            'NAME': 'Django',
                            'USER': 'root',
                            'PASSWORD': 'xxxxxxxxxxxx',
                            'HOST': '127.0.0.1',
                            'PORT': '3306',
                        }
                    }




    🔸 配置模块: settings.py > INSTALLED_APPS

        👁‍🗨 Django的模块是可插拔的(可启用也可禁用)。
            启用模块. 在INSTALLED_APPS 中添加 模块名就可以
            弃用模块. 在INSTALLED_APPS 中删除 模块名就可以

        ❗️❗️ 所有项目用到的模块必须在 INSTALLED_APPS 里添加模块名. ❗️❗️

        新建项目默认包含6个应用.以方便开发者使用。
        如果你安装了或者新建了一个模块, 那么必须在 INSTALLED_APPS 中添加新模块名. 
        比如用 python manage.py startapp article 新建了article模块, 
        就必须在 INSTALLED_APPS 添加 'article' ; 注意逗号. 

            INSTALLED_APPS = [
                'django.contrib.admin',        默认➜  管理站点，你能立即使用上
                'django.contrib.auth',         默认➜  认证系统
                'django.contrib.contenttypes', 默认➜  内容类型的框架
                'django.contrib.sessions',     默认➜  session框架
                'django.contrib.messages',     默认➜  messaging框架
                'django.contrib.staticfiles',  默认➜  静态文件管理

                'article'                      新增➜ 
                ]




    🔸 数据库表创建 migrate

        Django 是后端,肯定需要处理数据. 当然就需要用到数据库.
        Django 自带的模块也需要配合数据库才能使用.

        比如 Django 后台需要有帐号才能登录admin管理页面. 
        那就需要新建管理员账户, 账户信息当然是保存在数据库中的某个表里面了!
        所以要先配置数据库后才能使用Django.
        我们选择MySQL数据库来储存我们的Django资料.
        先去mysql里创建了一个叫Django的数据库. 
        然后还需要在Django数据库里创建一个数据库表后才能储存数据.

        ⦿ 创建数据表 python manage.py migrate

            migrate命令会查看INSTALLED_APPS的模块，
            然后根据 settings.py文件中的数据库设置来创建对应的数据库表。

        ⦿ 卸载模块
            先从INSTALLED_APPS中 删掉对应模块, 
                然后再用 python manage.py migrate 重新生成数据库表
                    migrate命令只会创建 INSTALLED_APPS中的模块需要的数据表.


    🔸 数据库表迁移 makemigrations
        python manage.py makemigrations appName
        通过运行makemigrations，你可以告诉Django你已经对你的model做出了一些改变，并且你希望这些改变可以作为migration存储起来。

        migration 迁移的意思

        ❗️❗️❗️ Django通过Migration来存储你的模型（也即是数据库对象）的变化。
        ❗️❗️❗️ Django通过Migration来存储你的模型（也即是数据库对象）的变化。
        ❗️❗️❗️ Django通过Migration来存储你的模型（也即是数据库对象）的变化。



    🔸 Django API

        Django提供的免费API, 让我们可以用命令来和Django 交互.
        
        ⦿ 进入交互界面:     python manage.py shell



    🔸 Django Admin介绍

        Django管理站点可以让你简单的添加，更改，删除网页内容.
        这个管理站点只能有网站管理员登录.

        ⦿ 创建管理员

            首先，我们需要创建一个用户来登录管理站点。使用下面的命令：
            python manage.py createsuperuser
            输入你想要的用户名
            Username: admin
            输入邮箱地址
            Email address: admin@example.com
            最后输入你的密码两次。
            Password: **********
            Password (again): *********
            Superuser created successfully.


    🔸 开发服务器

        想看看你的网站效果. 用自带的开发服务器最简单了.
        当然也可以用 web服务器如: apache、nginx....

        ⦿ 启动开发服务器:   python manage.py runserver
        ⦿ 查看网站后台效果: 浏览器 > http://127.0.0.1:8000/admin/




    🔸 创建页面:  new page/sub page ➜ Next ➜ 

    🔸 发布页面:  创建 不等于 发布,  要发布得点击 publish

    🔸 修改页面:  page ➜ page setting ➜ 

    🔸 structure/content 模式
        两种显示模式. 都可以对网页进行修改. 
        content 模式修改能力有限,一般用来看修改后的效果
        structure 模式 才是真正的修改模式..该模式下可以添加插件.

        ✚ ➜ 选择text ➜ 输入 ... 就多了...



    🔸 example.com

        pages: 进入页面编辑中心.

        disable toolbar: 关闭后怎么恢复.
            同一个网页.
            带toolbar的网址:  http://23.105.192.96:8000/en/?edit 
            无toolbar的网址:  http://23.105.192.96:8000/en/?toolbar_off

            所以只要修改浏览器网址 就可以切换有无toolbar了.


    🔸 page list
        列出页面结构.
        这里可以编辑删除 等等操作.


    🔸 项目静态文件.
        css、img、js、vendor



    🔸 中文乱码问题
        在.py文件的顶部添加以下代码
        # -*- coding: utf-8 -*-




🔵 INSTALLED_APPS

    'cms'，Django CMS本身；

    'mptt'，实现修正预排序遍历树的实用工具；

    'menus'，模型独立层次的网站导航助手；

    'south'，智能架构和数据迁移；

    'sekizai'，管理javascript和css。









🔵 Django-cms 

    Django-cms 官网:  https://www.django-cms.org/en/

    ❗️❗️❗️官方最新版教程(EN): http://docs.django-cms.org/en/latest/introduction/install.html


    ❗️❗️❗️ Django-cms 支持功能: https://www.django-cms.org/en/features/list/

    ⦿ content ➜ 双击文本区域可以直接编辑文章.

    ⦿ CMS Plugins 插件选择.. 不同的插件有不同的功能.


    有很多第三方插件!!! 来实现额外功能.
    比如SSL 证书、LDAP 登录验证、session 管理、拼写检查、文件上传下载、文本搜索、SASS支持、
    标签、标签云、评论、....




🔵 Django 简介&安装

    🔸 Django 安装(Centos6)
        先安装python 2.7+ ;在安装Django: pip install django==1.11.1
        
    🔸 Django 安装(Mac OS)

        ⦿ Python 版本 
            Diango 是基于Python的. 所以必须先安装Python; Mac 自带Python 2.7 

        ⦿ Diango 版本
            有很多版本. 选择长期支持的版本如: 1.11  最新版本是1.11.1  我们就下这个.

            github 官网:  https://github.com/django/django/releases
            branch ➜ tag: 1.11.1  ➜ download zip  ➜  然后解压
            🔅 cd diango 
            🔅 sudo python setup.py install

                👹 如果报错. 可能以及安装过diango.必须先删除旧版本然后重装
                rm -R /usr/local/Cellar/python/2.7.12/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/django

        ⦿ 安装测试
            只要上面安装不报错.基本就是安装成功的.
            安装好就可以在终端用 django-admin.py 这个命令了






🔵  DjangoCMS 环境搭建:


    🔸 Django-cms 虚拟环境搭建实例

        ⦿ 目的:  在MAC 桌面 创建一个名叫 mycms 的django-cms 项目.

        🔅 cd 到桌面
        🔅 virtualenv djcmsenv && source djcmsenv/bin/activate              
            创建 djcmsenv 的虚拟环境并进入. 你会发现前缀有变化

        🔅 pip install --upgrade pip         更新pip. 确保pip是最新版本
        🔅 pip install djangocms-installer   自动安装所有djangocms需要的依赖!!
        🔅 deactivate && source djcmsenv/bin/activate  ❗️❗️❗️必须重启虚拟环境

        🔅 djangocms mycms && cd mycms        用djangocms创建名为mycms的项目
            ⦿ 配置PostgreSQL数据库(可选)
                ⦿ 安装postgresql Python模块: pip install psycopg2
                ⦿ 配置 settings.py
                ⦿ .... 

            ⦿ 创建后台账户:                   python manage.py createsuperuser
            ⦿ 运行开发服务器:                 python manage.py runserver
            ⦿ 安装插件

        🔅 deactivate                        退出虚拟环境.
        🔅 rm -rf djcmsenv                   删除虚拟环境，只需删除它的文件夹.





🔸 PostgreSQL 数据库配置 ✔︎

    ⦿ 新建 PostgreSQL 数据库
        数据库名: DjangoCMS
        编码: UTF8


    ⦿ 安装   pip install psycopg2

    ⦿ settings.py 配置数据库

        DATABASES = {
            'default': {
                'CONN_MAX_AGE': 0,
                'ENGINE': 'django.db.backends.postgresql_psycopg2',
                'HOST': '127.0.0.1',
                'NAME': 'DjangoCMS',
                'PASSWORD': 'xxxxx',
                'PORT': '5432',
                'USER': 'postgres',
            }
        }


    ⦿ 创建数据表
    python manage.py migrate

    ⦿ 创建后台登录用户
    python manage.py createsuperuser




⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
🔵 PostgreSQL 版本升级

    版本升级:
        数据库里如果有数据麻烦点. 没数据先卸载再安装就可以了.我们选择卸载再重装
        官网:  https://www.postgresql.org/download/

🔸 Mac 

    ⦿ 卸载
        1、一般是使用installer图形界面方式安装的。(自带图形界面卸载程序的)

            👁‍🗨 如果是postgresql.app的形式，这个简单，跟其他app一样，删除app即可。

        2. open /Library/PostgreSQL/9.6/uninstall-postgresql.app
            这个就是打开卸载程序的方法.  9.6 更具你自己的版本修改

        3. 等待上一步执行完成后，删除postgresql文件夹       
            sudo rm -rf /Library/PostgreSQL

        4. 删除配置文件
            sudo rm /etc/postgres-reg.ini

        5. sudo rm /usr/local/bin/postgres

        6. 在用户管理中删除postgresql的用户， 系统偏好设置－－》用户及用户组。


    ⦿ 安装 

    下载链接1:
    https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#macosx

    下载链接2:   https://www.bigsql.org/postgresql/installers.jsp/


    ⦿ 环境路径设置
        Mac 版本查看很特别. 用系统终端 postgres -V 查出来的版本不准.
        type postgres 出来的可能是 /usr/local/bin/postgres

        需要设置 我用的zsh 就在修改 .zshrc 文件添加下面这行
        目的就是把 postgres 安装的路径加进去!
        export PATH=/Library/PostgreSQL/9.6/bin:$PATH
        然后重启zsh. 现在用 postgres -V 就出来9.6.3 了.
        ✘✘∙𝒗 Desktop type postgres
        postgres is /Library/PostgreSQL/9.6/bin/postgres



🔵 Postgresql 使用

安装好后. 默认生成一个 叫postgres 的数据库 和一个叫postgres的数据库用户.
同时还在系统里 生成了一个叫 postgres 的Linux系统用户

所有的操作 都要在 postgres 这个用户下进行!!! 

要在 postgres 里面添加新用户就要在系统里面添加新用户然后再设置.

比如我要新增一个叫 syncUser 用户.

sudo useradd syncUser  创建系统用户.
su postgres     切换到 postgres 用户
whoami          查看当前用户名 显示的是 postgres; 用 su root 切换回 root 用户
psql            进入 PostgreSQL 的命令控制台 系统提示符会变为"postgres=#"
\password postgres   使用\password命令，为postgres用户设置一个密码。

CREATE USER syncUser WITH PASSWORD 'syncUser';
创建叫 syncUser的数据库用户, 并设置好密码.
注意之前创建的是linux系统用户. 现在创建的是数据库用户..

CREATE DATABASE exampledb OWNER syncUser;
创建名为 exampledb 的数据库. 拥有者是syncUser 


GRANT ALL PRIVILEGES ON DATABASE exampledb to syncUser;
数据库的所有权限都赋予dbuser，否则dbuser只能登录控制台，没有任何数据库操作权限。


最后，使用\q命令退出控制台（也可以直接按ctrl+D）。




    ⦿ 初始化数据库

        默认是数据库数据存放位置在 /var/lib/pgsql/9.6/data下面.
        我们专门另外建个文件夹来存放数据.
        mkdir -p /opt/pgsql/data

        赋予postgres用户该目录的权限：
        chown postgres /opt/pgsql/data

        切换到postgres用户：
        su postgres

        执行初始化：
        initdb -D /opt/pgsql/data
            -D 指定数据库文件存放目录，
            如果不指定则默认在/var/lib/pgsql/9.6/data下


    ⦿ 启动服务


        1.切换到postgres用户
        su postgres
        这个步骤同样必须以PostgreSQL用户帐户登录来做。
        需要输入密码.
        忘记密码的话... 重置密码 
        用root   passwd postgres  就可以重置了.


        2.启动服务
        没有-D选项，服务器将使用环境变量PGDATA命名的目录； 
        如果这个环境变量也没有，将导致失败。
        通常，最好在后台启动postgres，使用下面的 Unix shell 语法：

        pg_ctl -D /opt/pgsql/data/ -l logfile start

        3.设置开机自动启动
        在Linux系统里，要么往/etc/rc.d/rc.local或 /etc/rc.local文件里加上下面几行：

        /usr/local/pgsql/bin/pg_ctl start -l logfile -D /usr/local/pgsql/data



    ⦿ 重启服务

        sudo /etc/init.d/postgresql-9.6 stop && /etc/init.d/postgresql-9.6 start

        sudo service postgresql-9.6 stop && service postgresql-9.6 start





    ⦿ 自动启动
        service postgresql-9.6 start
        chkconfig postgresql-9.6 on













    ⦿ 创建用户
        su postgres   切换到 postgres 用户.进行操作
        psql     进入控制台
        CREATE USER root WITH PASSWORD 'root';
        数据库中加完 root 后就可以在root用户下进行postsql的操作了.
        就不需要在切换到postgres用户下操作了.
        
        root用户下 直接 
        psql + 数据库名 就可以进入postgresql的命令行了
        如: psql postgres 就进去了.

    






    🔸 数据库操作
        \?：查看psql命令列表。

        \du：列出所有用户。

        \l：列出所有数据库。
        \d：列出当前数据库的所有表格。

        \c [database_name]：连接其他数据库。
        \d [table_name]：列出某一张表格的结构。


        \e：打开文本编辑器。
        \conninfo：列出当前数据库和连接的信息。



        基本的数据库操作，就是使用一般的SQL语言。
        # 创建新表 
        CREATE TABLE user_tbl(name VARCHAR(20), signup_date DATE);
        # 插入数据 
        INSERT INTO user_tbl(name, signup_date) VALUES('张三', '2013-12-22');
        # 选择记录 
        SELECT * FROM user_tbl;
        # 更新数据 
        UPDATE user_tbl set name = '李四' WHERE name = '张三';
        # 删除记录 
        DELETE FROM user_tbl WHERE name = '李四' ;
        # 添加栏位 
        ALTER TABLE user_tbl ADD email VARCHAR(40);
        # 更新结构 
        ALTER TABLE user_tbl ALTER COLUMN signup_date SET NOT NULL;
        # 更名栏位 
        ALTER TABLE user_tbl RENAME COLUMN signup_date TO signup;
        # 删除栏位 
        ALTER TABLE user_tbl DROP COLUMN email;
        # 表格更名 
        ALTER TABLE user_tbl RENAME TO backup_tbl;
        # 删除表格 
        DROP TABLE IF EXISTS backup_tbl;










⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
主从设置. 可以看成一种备份. 本地没了 服务器上数据还在.
主从的数据库版本最好一致!!  我们都是 9.6.3 
http://www.jianshu.com/p/2d07339774c0

Postgresql 主从配置   Mac 本地为主.  vps 为从...

Mac 是内网IP, VPS 是公网IP.
所以必须先给 Mac 配置个外网端口才行. 还是用 frp..
把23.105.192.96:2345 转到Mac的 5432端口来.

🔸 FRP 设置

    1. vps服务器启动frps  nohup /root/frp/frps -c /root/frp/frps.ini &
    2. mac本地启动frpc    nohup ./frpc -c ./frpc.ini &
    3. frp 网页控制台     http://www.0214.help:5500/

    4. 服务器配置frps.ini
        [postgresql]
        auth_token = 123
        listen_port = 2345

    5. 客户端配置frpc.ini
        [postgresql]
        auth_token = 123
        local_port = 5432

    6. 重启 服务器 frps:   
        ➜  pidof frps  
        ➜  kill xxx  
        ➜  nohup /root/frp/frps -c /root/frp/frps.ini &

    7. 重启 客户端 frpc:   
        ➜  pidof frpc  
        ➜  kill xxx 
        ➜  /Users/v/Desktop/frp/frpc -c /Users/v/Desktop/frp/frpc.ini &


    8. 测试
        用navicate 来测试.
        新建postgresql的连接.
        ip: 23.105.192.96
        端口: 2345 
        username Mac的数据库用户
        password mac的数据库密码

        然后测试, 能连上去 说明frp配置是没问题的!
        其实连接成功后去 frp 网页控制台 会看到 
        叫postgresql的服务. 有流量的. 说明正在工作



🔸 Mac 主服务器设置

    ⦿ 进入 postgresql 命令行
        psql -U postgres
        输入密码就进去了


    ⦿ 终端切换用户
        su PostgreSQL
            这里用户名自己去系统设置 下的用户里面看

    ⦿ 服务命令
        必须进入 postgres 用户下操作 
        /Library/PostgreSQL/9.6/bin/pg_ctl restart -D /Library/PostgreSQL/9.6/data

        /Library/PostgreSQL/9.6/bin/pg_ctl stop -D /Library/PostgreSQL/9.6/data

    ⦿ 添加用户 (Navicast) 


    ⦿ 配置 pg_hba.conf， 来允许用户来同步

        su postgres     切换用户 需要输入 postgres用户的密码.不是root的密码
        vi /Library/PostgreSQL/9.6/data/pg_hba.conf
        添加下面行
        host    replication     postgres        0.0.0.0/0         md5
        意思就是允许vps的ip 上的 postgres 用户 来本机进行同步...


    ⦿ 配置 postgresql.conf

        vi /Library/PostgreSQL/9.6/data/postgresql.conf

        listen_addresses = '*'   # 监听所有IP
        archive_mode = on  # 允许归档
        archive_command = 'cp %p /opt/pgsql/pg_archive/%f'  # 用该命令来归档logfile segment
        wal_level = hot_standby 
        max_wal_senders = 32 # 这个设置了可以最多有几个流复制连接，差不多有几个从，就设置几个wal_keep_segments = 256 ＃ 设置流复制保留的最多的xlog数目
        wal_sender_timeout = 60s ＃ 设置流复制主机发送数据的超时时间
        max_connections = 100 # 这个设置要注意下，从库的max_connections必须要大于主库的


    ⦿ 测试
        然后去 vps 上看看能不能连上 mac的数据库.
        服务器终端切换到 postgres 用户
        psql -h 192.168.20.93 -p 2345 -U postgres
        这样就进入 Mac 的数据库了!!!

        查看 psql 命令用法.
        psql --help

        -p 指定端口
        -h 指定主机名
        -U 指定用户名


🔸 vps 从服务器配置

    ⦿ 基础备份: 先主服务器 拷贝数据到从服务器.

        1. 切换用户  su postgres

        2. 清空从服务器的所有数据  rm -rf /opt/pgsql/data/*

        3. 复制主服务器的数据
        pg_basebackup -h 23.105.192.96 -p 2345 -U postgres -D /opt/pgsql/data -X stream -P

        然后输入mac的 postgres 账户的密码 就开始同步了!

        pg_basebackup --help
        -h 主机
        -p 端口
        -U 数据库用户名
        -D 数据库资料文件夹 不是数据来源文件夹. 是数据备份文件夹
        -X 数据传输方法??
        -P 显示进程信息



⦿ 备份方法2 

pg_basebackup -h 23.105.192.96 -p 2345 -U postgres -F p -x -P -R -D /opt/pgsql/data -l replbackup2017

cd /opt/pgsql/data && ls

下面简单做一下参数说明（可以通过pg_basebackup --help进行查看），-h指定连接的数据库的主机名或IP地址，这里就是主库的ip。-U指定连接的用户名，此处是我们刚才创建的专门负责流复制的repl用户。-F指定了输出的格式，支持p（原样输出）或者t（tar格式输出）。-x表示备份开始后，启动另一个流复制连接从主库接收WAL日志。-P表示允许在备份过程中实时的打印备份的进度。-R表示会在备份结束后自动生成recovery.conf文件，这样也就避免了手动创建。-D指定把备份写到哪个目录，这里尤其要注意一点就是做基础备份之前从库的数据目录（/usr/local/postgresql/data）目录需要手动清空。-l表示指定一个备份的标识，运行命令后看到如下进度提示就说明生成基础备份成功：











    ⦿ 配置 recovery.conf

        1.) 复制/usr/pgsql-9.6/share/recovery.conf.sample 到 /opt/pgsql/data/recovery.conf
        cp /usr/pgsql-9.6/share/recovery.conf.sample /opt/pgsql/data/recovery.conf

        2.) 修改 recovery.conf
        vi /opt/pgsql/data/recovery.conf

        standby_mode = on    # 说明该节点是从服务器

        primary_conninfo = 'host=23.105.192.96 port=2345 user=postgres password=postgres'

        recovery_target_timeline = 'latest'



    ⦿ 配置postgresql.conf
        vi /var/lib/pgsql/9.6/data/postgresql.conf

wal_level = hot_standby  加这行
max_connections = 1000   改这行 原来是100. 改成大于100就行 
hot_standby = on         加这行 ＃ 说明这台机器不仅仅是用于数据归档，也用于数据查询
max_standby_streaming_delay = 30s    去注释 # 数据流备份的最大延迟时间
wal_receiver_status_interval = 10s   去注释 # 多久向主报告一次从的状态，当然从每次数据复制都会向主报告状态，这里只是设置最长的间隔时间

hot_standby_feedback = on 加这行 # 如果有错误的数据复制，是否向主进行反馈



⦿ 重启
su postgres 
pg_ctl stop -D /opt/pgsql/data
pg_ctl start -D /opt/pgsql/data

pg_ctl stop -D /var/lib/pgsql/9.6/data
pg_ctl start -D /var/lib/pgsql/9.6/data

到底是哪个路径呢 ...
postmaster.pid
服务正常运行才会生成postmaster.pid  如果服务不正常是没有.pid的 !!!
所以现在问题肯定是服务没起来!  那么就去启动服务看看有什么错误.
[postgres@mail]/opt/pgsql/data% pg_ctl start -D /opt/pgsql/data
server starting
[postgres@mail]/opt/pgsql/data% 2017-06-19 00:01:34 CST LOG:  could not bind IPv4 socket: Address already in use
2017-06-19 00:01:34 CST HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
2017-06-19 00:01:34 CST LOG:  could not bind IPv6 socket: Address already in use
2017-06-19 00:01:34 CST HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
2017-06-19 00:01:34 CST WARNING:  could not create listen socket for "*"
2017-06-19 00:01:34 CST FATAL:  could not create any TCP/IP sockets
2017-06-19 00:01:34 CST LOG:  database system is shut down

据说.. 已经有个服务运行在 5432端口上了... 我们看看
ps 发现有个 postgre 进程 kill掉. 再启动 就起来了..

👹 incorrect checksum in control file
操..主服务器是64位的... 从服务器是32位的.... 难道是这个原因坑.....

❗️ google 这个问题  Replication between 64/32bit systems?
https://www.postgresql.org/message-id/003401cc77d5%2484828f30%248d87ad90%24%40com.br

发现...主从同步, 双方的系统最好一样,环境最好一样.
也就是说.主是32位系统.从就绝对不能是64位系统. 
主是Mac os 从好像就不能是windows/linux...限制多...

看来用postgresql 在mac 和 vps 之间主从是没办法实现了. 
看来... 博客数据还是直接用sqlite吧. 备份也省力. 复制文件夹就可以了.

丫的... 难道本地搭建个 docker 整天运行不成... 这样..mysql/postgre 主从都能实现了.
还是用虚拟机 搭建个 centos6 ..
docker 玩玩可以 实际要用.. 麻烦..
那就虚拟机吧.. 弄个centos6 x86的镜像. 最小化安装...
或者 再买个VPS!!!  反正自己两个IP. 







⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
mysql 主从...

Mac: mysql 5.7.14 x64
VPS: mysql 5.1.73 x86




















    🔸 Mysql 数据库配置 ✗

        ⦿ 数据库配置流程: 
            ➜ 选择数据库: mysql 
            ➜ 创建数据库: djcmsDB > utf8 > utf8_bin 
                ➜ 配置settings.py 
                ➜ 创建数据表
                    ➜ 创建登录用户 
                    ➜ 登录测试

        ⦿ 选择数据库.

            用mysql         就先安装mysql.     为了方便Python使用 mysql需安装 pip install mysql-python
            用postgresql    就先安装postgresql 为了方便Python使用 postgresql需安装 pip install psycopg2
            用sqlite (默认自带) 就什么都不用设置.

        ⦿ 创建数据库: djcmsDB
            如果你用 sqlite 这个默认的数据库Django 会自动帮你建立数据库.
            如果你用 sqlite 之外的的数据库就得手动建立数据库.


        ⦿ 安装  MySQL-python
            虚拟环境需要重新安装 MySQL-python !!! 
            但是 mac电脑在虚拟环境中直接安装MySQL-python会报错.
            需要先运行  xcode-select --install
            再          pip install MySQL-python



        ⦿ 配置settings.py
            如果你用 sqlite 这个默认的数据库Django 会自动帮你配置数据库信息.
            如果你用 sqlite 之外的的数据库就得手动去settings.py 中配置数据库信息.


            - 配置 /CMS/mycms/settings.py 里的DATABASES 成如下:
                👁‍🗨 注意'HOST': '127.0.0.1' 绝对不能用  'HOST': 'localhost' 一个是域名一个是ip.
                👁‍🗨 先在终端用 mysql -u root -p  测试mysql用户名和密码是否正确.

                vi /CMS/mycms/settings.py
                            DATABASES = {
                                'default': {
                                    'CONN_MAX_AGE': 0,
                                    'ENGINE': 'django.db.backends.mysql',
                                    'HOST': '127.0.0.1',
                                    'NAME': 'djcmsDB',
                                    'USER': 'root',
                                    'PASSWORD': 'xxxxxxxxxxxx',
                                    'PORT': '3306',
                                }
                            

        ⦿ 创建数据表
            cd /CMS && python manage.py migrate

            数据库必须先建立一系列数据表后才能用.
            建立数据表后. 就能用浏览器访问 23.105.192.96:8000 了. 会自动跳转到后台登录页面.
            想要登录就必须先创建登录帐号.


        ⦿ 创建登录用户.
            cd /CMS && python manage.py createsuperuser


        ⦿ 登录测试 23.105.192.96:8000








    🔸 设置时区
        sudo vi mycms/settings.py 
        TIME_ZONE = 'xxx'   改成  TIME_ZONE = 'Asia/Shanghai'





    🔸 ALLOWED_HOSTS 设置
        项目创建好就可以运行项目了. 
        由于我们是ssh到服务器搭建的Django项目. 不是在本地搭建的. 所以有额外多了这一步.
        本地搭建项目的 可以省略 这一步.


        sudo vi /CMS/mycms/settings.py 
            ALLOWED_HOSTS = [] 行改成:
            ALLOWED_HOSTS = ["23.105.192.96", "www.0214.help"] 

        👁‍🗨 sed 一键修改命令:
        sed -i '/ALLOWED_HOSTS/c ALLOWED_HOSTS = ["23.105.192.96", "www.0214.help"] ' /CMS/mycms/settings.py




    🔸 启动开发服务器

        cd /CMS/mycms && python manage.py runserver 0.0.0.0:8000

        执行上面命令.然后浏览器访问 23.105.192.96:8000  就会出现 django CMS的登录页面了.
        当然现在你还登录不了. 不要急.得创建用户.
        至于怎么创建用户 就和Django 创建用户的方法一模一样了. 
        由于我们要使用mysql.而不是Django自带的 sqlite. 
        我们得先创建Mysql数据库.然后配置 /CMS/mycms/settings.py 中的数据库配置.










🔵 常用命令

    ⦿ 创建项目:  cd /home && django-admin startproject djSite
    ⦿ 创建模块:  python manage.py startapp polls

    ⦿ settings.py mysql 数据库配置:
            sed -i "s/django.db.backends.sqlite3/django.db.backends.mysql/g; s/'NAME': os.path.join(BASE_DIR, 'db.sqlite3')/'NAME': 'djSiteDB'/g" /home/djSite/djSite/settings.py &&  sed -i "/'NAME': 'djSiteDB'/a \ \n\t'USER': 'root',\n\t'PASSWORD': 'xujian0219',\n\t'HOST': '127.0.0.1',\n\t'PORT': '3306', " /home/djSite/djSite/settings.py


    ⦿ 开启外网权限:
        sed -i '/ALLOWED_HOSTS/c ALLOWED_HOSTS = ["23.105.192.96", "www.0214.help"] ' /home/djSite/djSite/settings.py


    ⦿ 建立数据库表          cd /home/djSite/ && python manage.py migrate


    ⦿ Admin 创建账户:       python manage.py createsuperuser
    ⦿ 运行开发服务器:       python manage.py runserver 0.0.0.0:8000
    ⦿ 查看网站首页:         127.0.0.1:8000 


    ⦿ 清空数据库,只留表     python manage.py flush
    ⦿ Django API:           python manage.py shell
    ⦿ Django 版本查看:      python -m django --version  或者 pip list django


    






🔵 本文目的:

    ⦿ 闲聊
            看了 127.0.0.1:8000 显示出来的页面. 好丑. 
            肯定想要大修大概重新设计一个网站.这就是基于djangocms的二次开发.

            我们为什么要基于djangocms来设计一个网站. 而不是完全从零开始设计一个网站呢.这是为了减少开发难度. 
            从0开始设计网站很麻烦的. 前端技术范围非常广,要学的东西非常非常多.
            我之前干的运维.前端完全是自学的.花费了我几乎半年时间才有了这个网站: www.0214.live ,
            看着没啥功能,但是任何功能的实现都是自己想出来的.而不是引用现成的模块什么的.
            其实没有太大的必要这样做.太费时间了.我估计你也不想花半年时间才能做出一个不怎么样的网站吧.

            用 Python 搭建网站的好处就是 有着非常非常多的很优秀的模块.
            这些模块都是很多高手设计出来的,肯定比你自己设计出来的好很多很多.
            我们要做的就是引用这些优秀的模块.然后稍加修改就可以做出一个非常优秀的网站.
            这就是所谓的 站在巨人的肩膀上.... .... 

            废话不多说.. 我们先挑一个好看点的网站来做参照.看看我们能不能作出一样的效果.
            这是我选的参照网站: https://blackrockdigital.github.io/startbootstrap-modern-business/
            我们的目的就是把djangocms 默认显示的网站改成类似上面的.

    ⦿ 参考网站 架构分析
        首先我们得分析参考网站的架构. 看看参考网站是怎么设计出来的.
        打开这个网站 ➜  https://builtwith.com/  ➜ 输入参考网站的网址 ➜ lookup ➜ 就会出来这个网站的架构
        我们简单分析下这个参考网站.
        主要就是用了: HTML5、jQuery、CSS、Bootstrap...


    ⦿ 参考网站 源码分析
        一个网站. 最重要的就是 html源码.
        我们想要设计出类似参考网站效果的网页. 就必须研究下网页源码.
        浏览器 F12 就可以查看源码.  我们先来看 index.html 首页的源码.不多也就300多行.


    ⦿ 参考网站 源码下载
        https://startbootstrap.com/template-overviews/modern-business/

        解压后会发现有很多文件:
        js/css/fonts/font-awesone/404.html、about.html、index.html....



⦿ Djangocms 源码分析
    Djamgocms 用的是模板!
    那么 127.0.0.1:8000/en/?edit 这个djangocms首页肯定是好几个模板.html + 数据库中数据 组合才形成的
    模板文件夹下有很多 html文件. 那么具体用了哪几个html 就要分析 views.py了





🔵 第三方模块安装



    🔸 bootstrap3 安装教程
        http://aldryn-boilerplate-bootstrap3.readthedocs.io/en/latest/

        1. pip install aldryn-bootstrap3
        2. 添加 'aldryn_bootstrap3', 到 INSTALLED_APPS
        3. 创建数据表  python manage.py migrate
        4. 随便找个 前端页面. 进入 structure模式. 添加插件. 就会多出bootstrap了.



    🔸 news & blog 安装教程
        http://aldryn-newsblog.readthedocs.io/en/latest/

        ⦿ pip install aldryn-newsblog

        ⦿ 去settings.py 添加一系列的 插件名.

            'aldryn_apphooks_config',
            'aldryn_categories',
            'aldryn_common',
            'aldryn_newsblog',
            'aldryn_people',
            'aldryn_reversion',
            'aldryn_translation_tools',
            'parler',
            'sortedm2m',
            'taggit',
            'reversion',


        ⦿ 同步数据库..python manage.py migrate
            👹 django.db.utils.OperationalError: (1553, "Cannot drop index 'aldryn_peopl_group_id_3944d5eea384fb80_fk_aldryn_people_group_id': needed in a foreign key constraint")

            不能删除某个索引index   因为这个索引被某个东西当成foreign key 了.

            问题处理方法: https://code.djangoproject.com/ticket/24653
            太麻烦..算了.. 不用mysql了.  还是用自带的 sqlite吧.
            要不试试 postsql




🔵  
环境搭建好了. 插件也安装好了. 下面就来



        ⦿ 使用
        安装好后..需要至少有一页的 page > advance settings > application settings >选择 newsblog

        然后在这个页面创建 news/blog article
        就可以建立博客了.  然后网址什么的 都不用你操心.
        博客是好了.但是... 网站必须有首页啊. 联系页面什么的! 
        我们先来看首页.. 
        新的 djangocms 项目 首页是空空荡荡的. 怎么才能改首页呢.
        就要去 mycms/templates 文件夹里面看了.
        默认有 4个 html.
        base 、fullwidth、sidebar_left、sidebar_right.
        其中 base 是最最基础的模板. 任何网页都会加载这个模板里的内容.
        如果你在base的body里面 添加 哈哈两个字. 那么所有的网页都会多出哈哈两个字.

        fullwidth、sidebar_left、sidebar_right 这三个是二级模板! 可以看成基础模板上再套用模板
        基础模板: base.html 适合用 网页版权! 还有网页顶部的导航栏目.
        二级模板: 可以看成网站的主题. 换模板就是换网站风格
        每个页面都可以设置单独的模板.

        djangocms自带的 基础模板太丑..  我们换掉..
        这个涉及前端知识.  我们直接用别人现成的网站. 然后修改下就可以了.

参考这个网站的布局.
https://blackrockdigital.github.io/startbootstrap-modern-business/index.html

网站源码下载:
https://startbootstrap.com/template-overviews/modern-business/


静态文件: 

        ⦿ css、font-awesone、fonts、js 文件夹 ➜ 移动到 desktop/mycms/mycms/static下
            👁‍🗨 这里注意. 不是移动到desktop/mycms/static文件夹下!!!

⦿ 把 full-width.html 移动到 desktop/mycms/mycms/templates 下
⦿ 删除 desktop/mycms/mycms/templates 下的 sidebar_left 和 sidebar_right 和base.html
⦿ 把 full-width 改成 base.html

接下来的东西 需要了解点 djangocms的模板知识. 不知道跟着做就可以了..

整个base.html 改成下面这样

{% load cms_tags menu_tags sekizai_tags staticfiles i18n %}
<!--为了使用模板的基本功能. 首先需要在最上方添加这行-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">

    <!-- 模板里调用静态文件 用下面的格式.-->
    <link href="{% static 'css/bootstrap.min.css' %}" rel="stylesheet">
    <link href="{% static 'css/modern-business.css' %}" rel="stylesheet">
    <link href="{% static 'font-awesome/css/font-awesome.min.css' %}" rel="stylesheet" type="text/css">

    <!--引入 djangocms 以及各种插件 所依赖的css文件-->
    {% render_block "css" %}

    <!--模板的标题-->
    <title>{% page_attribute 'page_title' %} - Xu.Jian </title>
</head>

<body>
    <!--cms 隐藏工具条-->
    {% cms_toolbar %}


    <!-- 网页导航条 -->
    <nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
        <div class="container">

            <!-- Brand and toggle get grouped for better mobile display -->
            <div class="navbar-header">
                <!--home 按钮-->
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="/">Home</a>
            </div>

            <!--导航条中的 home之外的按钮.-->
            <!-- Collect the nav links, forms, and other content for toggling -->
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav navbar-right">
                {% show_menu 0 100 100 100 %}
                </ul>
            </div>
            <!-- /.navbar-collapse -->
        </div>
        <!-- /.container -->
    </nav>


    <!--下面是网页的内容区域.  block xxx 就代表模块,  可以在模板里面用别的模板-->
    {% block content %}{% endblock %}

 
    <!--可以在 结构中 添加一个cate 区域.-->
    {% placeholder Cate %}
    {% placeholder Tage %}
    {% placeholder File %}



    <!-- Footer -->
    <footer>
        {% static_placeholder "Footer"%}
    </footer>
        
    <!-- jQuery -->
    <script src="{% static 'js/jquery.js' %}"></script>

    <!-- Bootstrap Core JavaScript -->
    <script src="{% static 'js/bootstrap.min.js' %}"></script>
        {% render_block "js" %}

</body>

</html>





🔸 重启开发服务器.
现在你看到的首页就变了!!  什么内容都没有.但是有个漂亮的导航条.
现在你点导航条中 所有的链接都是一模一样的网页. 除了nb 页面.
这个页面是和 news&blog 第三方插件绑定的. 这里可以创建博客. 我们先不管.
我们先做一个排列的 home 页面出来.
home 页面是由 base.html + 模板 组成的.
模板默认使用 settings.py > CMS_TEMPLATES 中的第一个元素.
CMS_TEMPLATES = (
    ('fullwidth.html', 'Fullwidth'),
    ('sidebar_left.html', 'Sidebar Left'),
    ('sidebar_right.html', 'Sidebar Right')
)
也就是名为 fullwidth 模板. 源文件是 fullwidth.html
我们来看 fullwidth.html
{% extends "base.html" %}
{% load cms_tags %}

{% block title %}{% page_attribute "page_title" %}{% endblock title %}

{% block content %}
	{% placeholder "content" %}
{% endblock content %}


就这么几行.  
{% extends "base.html" %} 大概意思这个是基础模板的扩展.
{% block content %}  是和 base里的 {% block content %}  对应的.
也就是说. 二次模板里的{% block content %} 里的内容会填充到 基础模板的{% block content %}里面.

大概意思就是.  基础模板 放一个  {% block content %} 标记.这标记里面什么内容都没有.
然后你可以有很多二级模板.
比如 left.html  right.html top.html
然后每个二级模板里面都可以有 {% block content %} 标记. 但是这里的标记里面是有内容的!
而且每个二级模板里的 {% block content %} 里的内容可以不一样.
这样 同样的首页. 
当你使用 left.html 模板时候就会显示 left.html 里的内容.
当你使用 right.html 模板时候就会显示 right.html 里的内容.
这样就达到目的了...

当然你可以给 home 单独设置一个 home.html 模板. 然后 home 的page设置里面选择home这个模板. 这样你的home 就达到自定义的效果了. 虽然这个home.html 只用在一页上(home页)


🔸 home.html
{% extends "base.html" %}
{% load cms_tags %}

{% block content %}
xx
	{% placeholder "content" %}
{% endblock content %}

然后 去settings.py  CMS_TEMPLATES 添加  ('home.html', 'Home'),

然后去首页 page 模板 选择 home  然后刷新home页.
是不是出现 xx 两个字了. 说明成功了.

下面我们来完善homepage.
这里介绍下placeholder 这是个区域! 这个区域里你可以添加各种插件 
非常强大..  我们现在的 home.html 里面有个 叫 content的placeholder
所有我们可以在 结构栏里面看到一个 Content的模块.
这里可以添加各种插件...

🔸 style 
    添加容器 container.  
    style ➜ container + div ➜ save


🔸 row 
    行.  一般是分成一列.
    但是下面的 设备什么意思. 
    把屏幕宽度 等分成12分. 
    填12 就说明这row 会占据整个屏幕宽度的 全部.
    填6  就说明这row 会占据整个屏幕宽度的 1/2.  这时候一行就可以并列放两个row了.

    当你用手机时 一般一行一个row
    当你用电脑时 一行可以放好几个row. 充分利用空间.


🔸 image
图片


🔸 text 
文本





这些设计不重要 . 重要的是 这些数据.
项目改来改去 很任意出错 就需要新建项目.. 里面的数据都是在数据库中 肯定有办法
可以快速的从之前的数据库 导出数据.. 并新建项目..

🔵 数据库迁移.

🔸 数据迁移 简单.
django 项目提供了一个导出的方法 python manage.py dumpdata, 

不指定 appname 时默认为导出所有的app
python manage.py dumpdata [appname] > appname_data.json

比如我们有一个项目叫 mysite, 里面有一个 app 叫 blog ,我们想导出 blog 的所有数据
python manage.py dumpdata blog > blog_dump.json

数据导入,不需要指定 appname
python manage.py loaddata blog_dump.json



🔸 数据迁移 复杂 






🔵  博客网页设计
自带的博客太丑了..  news&blog 的模板在哪呢...
在虚拟环境里
env/lib/python2.7/site-packages/aldryn_newsblog/templates/include 下面.
这个 就要看官方的教程了. http://www.mumuo.cn/2017/03/18/124/

首先是 article.html

用下面的 
<div class="post-preview">
    <a href="post.html">
        <h2 class="post-title">
            Man must explore, and this is exploration at its greatest
        </h2>
        <h3 class="post-subtitle">
            Problems look mighty small from 150 miles up
        </h3>
    </a>
    <p class="post-meta">Posted by <a href="#">Start Bootstrap</a> on September 24, 2014</p>
</div>
<hr>

代替 
{% if not article.published %} unpublished{% endif %}">
和
{% if detail_view %}
里面的内容












🔵 首页 添加最新博客插件
latest articles 这个插件 可以直接在首页 显示最新博客.

还可以添加分类!!!  标签等等.... 


到这里 博客就差不多了... 可以正式来了..
为了保证数据... 
项目定期备份 加上 数据库定期备份 就没问题. 

问题是搭建在本地上面 还是搭建到服务器上.还是两边都搭建...
或者先在本地... 然后数据同步到服务器..
项目同步...不难..但是数据库同步...好像可以实现.. 

服务器上 mysql是 5.1.73  本地是5.7.14 ...
据说跨版本也是可以实现的.




















⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️



🔵  新建简单模块

🔸 新建模块  python manage.py startapp Module1

🔸 配置模块 INSTALLED_APPS 添加 'Moudule1'



🔸 配置 Module1/views.py
目的: 在网页中显示 欢迎光临 四个字.

# -*- coding: utf-8 -*-

from django.shortcuts import render

from django.http import HttpResponse


def index(request):
    return HttpResponse(u"欢迎光临")



🔸 配置 mycms/urls.py
    目的: 把 module1 模块里面的views 导入到项目总url里.

    1. 添加行  from Module1 import views as Module1_views 
    2. urlpatterns 里添加  url(r'^en/Module1', Module1_views.index), 
    3. 浏览器访问 http://127.0.0.1:8000/en/Module1/  就可以看到 欢迎光临了.

所以要添加一个网页. 也就是 先写view.  然后配置urls 就差不多了.
这只是 最简单的模块. 下面来复杂一点的.


🔵 新建复制模块.
就比如说博客模块吧.

🔸 创建模块  python manage.py startapp blog

🔸 配置模块 INSTALLED_APPS 添加 'blog',

🔸 配置模型 blog/models.py
全部改成如下

from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title


🔸 创建模型数据表
    类似git 先commot 再同步

    python manage.py makemigrations && python manage.py migrate


🔸 管理后台 blog/admin.py
每个模块 都有自己的 后台!

# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.contrib import admin
from .models import Post

admin.site.register(Post)

🔸 登录后台  http://127.0.0.1:8000/en/admin/
就可以看到多出了一个 Blog Posts 
在这后台里面可以添加 各种博客..
当然最好是在前端可以添加博客. 
如果这个是django项目.而不是 djangocms项目.
那么浏览器访问 127.0.0.1:8000 就可以添加博客了.
但是现在的 127.0.0.1:8000 是djangocms的首页. 不是blog这个app的首页.

🔸 后台创建几篇文章
post1、post2...
创建好后 怎么才能在前端网页看到 新建的post呢..
好像需要用到 apphook.. 把app和djangocms结合起来..

搜索 django cms app integration





















⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️


🔸 mycms/settings.py 

    ⦿ CMS_TEMPLATES = (
        ## Customize this
        ('fullwidth.html', 'Fullwidth'),
        ('sidebar_left.html', 'Sidebar Left'),      ➜ 我们删除这行. 只留一个fullwidth模板就够了
        ('sidebar_right.html', 'Sidebar Right')     ➜ 我们删除这行. 只留一个fullwidth模板就够了
    )

    这里就是 模板了!!!  模板什么意思呢. 就是一个网站可以有好几种页面排版! 一个模板可以看成一个主题!!!
    用到的模板都需要写到这里 然后就可以在首页的 edit page ➜ page ➜ templates ➜ 有三种模板可选.(删除上面两行之后就只有一种选择了)
    我们为了简单. 只用一种模板. 就是全屏模板. 模板设计不简单的.设计一个模板几乎就是重新设计网站了.
    如果这里有多个模板. djangocms 会默认使用第一个当成默认的模板!!!




    我们去看看mycms的模板文件夹下 到底有什么文件: 

    ✘✘∙𝒗 mycms tree templates
    templates
    ├── base.html
    ├── fullwidth.html
    ├── sidebar_left.html  ➜ 删除这个文件. 因为我们用不到这个模板
    └── sidebar_right.html ➜ 删除这个文件. 因为我们用不到这个模板

    注意 base.html 是所有模板都用到的基础文件.  


🔸 fullwidth.html 文件内容: 
        {% extends "base.html" %}
        {% load cms_tags %}

        {% block title %}{% page_attribute "page_title" %}{% endblock title %}

        {% block content %}
            {% placeholder "content" %}
        {% endblock content %}

    - sidebar_left.html 和 sidebar_right.html 和fullwidth.html 差不多都是 {% extends "base.html" %} 开头的.
    这说明 fullwidth、sidebar_left、sidebar_right 都是基于 base.html 的.
    如果说 base.html 是个人. 那么模板就是面具. 带不同的面具 就有了不同的样子. 就这个意思





🔵 用参考网站中的 full-width.html 代替djangcms自带的 base.html
替代后. 再稍微修改下 我们的djangocms首页就会看起来和参考网站差不多了.
现在就得分析 base.html了.  我们要把 base.html中 属于模板的代码都挑出来 放到 full-width.html中.









⦿ 删除文件: base.html 直接删除
⦿ 重命名:   full-width.html 改成 base.html
现在我们用的模板/主题是 fullwidth.html . 而fullwidth.html 第一行引入的文件名是 {% extends "base.html" %}
我们想要用 模板网站的full-width.html 代替默认的base.html  就需要把 full-width.html 改名成base.html

⦿ 现在我们可以刷新 127.0.0.1:8000 网页了
应该和之前的网页变化很多了. 
这步如果出错. 用浏览器的F12 来排错. 


现在 点击网页右上角的三角形 就可以显示隐藏 django自到的工具栏了.
下面我们用 django 工具栏中的 create 添加几篇文章 如: home about 
然后这个 home about 就会显示在 bootstrap的工具栏目中.


其实 bootstrap 工具栏上显示的内容是在 django工具栏➜ example.com ➜ pages ➜ 有个 menu 选项.
打勾 就显示在 工具栏上.  不打勾就不显示在工具栏上.



🔸 修复 工具栏上的 start bootstrap 按钮的链接.
默认是链接到 /en/index.html的. 但是我们django的主页 默认是

编辑模板下的 base.html
                <a class="navbar-brand" href="index.html">Start Bootstrap</a>
➜                 <a class="navbar-brand" href="/">Start Bootstrap</a>





🔸 模板首页.
https://blackrockdigital.github.io/startbootstrap-modern-business/index.html
我们发现 模板首页和 模板其他页面是有明显区别的!!!
模板首页估计没有用基础模板, 是单独的一个网页.
而我们现在的 项目网站 所有网页都是一个模板的.

要给home 单独一个页面. 我们就得大概了解下 django模板的运行原理.

首先看mycms网站的组成吧

顶部导航栏(Nav) + 内容(page content) + 页脚(footer)



首先 mycms项目 所有网页都需要顶部的工具栏+页脚. 这个是肯定的.
但是 内容这块就不一定了.  首页的内容和非首页的内容肯定不一样.
base.html 中的 footer 是包含在 page content 标签中的. 
首先我们就要把footer 从 page content 中移出来.

🔸 页脚 移出 <page content> 外.  


所以 内容这块就不能写到基础模板(base.html)里.因为base.html 所有网页都会载入的模板.

那么内容这块怎么办呢! 




🔸 替换 page content 

把 <!-- Page Content --> 里的所有内容替换成:
{% block content %}{% endblock %}
👁‍🗨 这行的意思就是.  无论你什么时候看到 一个脚 content的 block. 
就用 一个二级基础模板 来代替 {% block content %}{% endblock %} .
也就是说 {% block content %}{% endblock %} 可以看成一个二级基础模板.
base.html 本身就是一个基础模板


fullwith.html 里面也有 一个 block content.  

{% block content %}
	{% placeholder "content" %}
{% endblock content %}


简单说. 	{% placeholder "content" %} 
base.html 中  {% block content %}{% endblock %} 这行会被 fullwidth.html中的 {% placeholder "content" %} 取代.

{% placeholder "content" %} 到底是什么 以后再说...









    </nav>

    {% block content %}{% endblock %}
    
    <!-- Footer -->
    <footer>







基础模板是 base.html!!!!!!





🔵 模板
http://docs.django-cms.org/en/latest/how_to/templates.html


所有django cms 网页的基础模板就是 base.html
所有网页首先加载的肯定是 base.html








默认使用 settings.py cms_templates 中的第一个当模板!!!

两种模板标记:
placeholder 


现在 about 和 home 页面不一样了.. 





🔸 模板继承
http://support.divio.com/academy/bonus-topics/understanding-template-inheritance

❗️❗️❗️ 其实一个模板里面还可以用 另外一个模板. 用block来扩展就可以了.

所有的网页 都会有一样的头和脚. 但是内容肯定是不一样的.
但是作为博客. 所有文章的内容格式都素一样的. 只
只有个别home、about、等等页面的格式 和 文章的内容格式不一样.

我们还是有必要写个 content.html 模板给文章用!!! 
至于home、about页的内容 只能单独写一个html页面了.



base.html 一般写网页的 头和脚.  那么内容怎么办呢.
content.html 下面这样写就可以了

{% extends "base.html" %}
{% load cms_tags %}

{% block content %}    
    {% placeholder content or %}        
        <p>This page has no content yet. Make sure you are in 
        <em>Edit</em> mode (hit the <strong>Edit page</strong>  
        button if required). Then switch to <em>Structure</em> mode.</p>
{% endplaceholder %}{% endblock content %}








🔵  DjangoCMS 储备知识

    🔸 模板标记

    ⦿ {% load cms_tags menu_tags sekizai_tags staticfiles %} 
                                ➜ 必须放模板文件第一行! 说明这是一个模板
                                ➜ 注意 staticfiles 表示模板里可以用静态文件

    ⦿  <title>{% page_attribute 'page_title' %} - Xu.Jian </title>
        网站标题

    ⦿ {% cms_toolbar %}         ➜ 顶部工具栏.
                                ➜ 一般放到 <body> 下面一行


    ⦿ {% show_menu 0 100 100 100 %}  ➜ 菜单条 
                一般放在导航栏里面的<ul></ul> 标签里面

            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav navbar-right">
                {% show_menu 0 100 100 100 %}
                </ul>
            </div>         



    ⦿ {% render_block "css" %}  ➜ 加载 djangocms 以及所有插件依赖的 css 
                                ➜ 一般放到 <head> 上面一行

    ⦿ {% render_block "js" %}   ➜ 加载 djangocms 以及所有插件依赖的 js
                                ➜ 一般放到 </body> 上面一行




    🔸 静态文件路径





    🔸 静态文件引用
        css、js 都需要改成模板标记 然后Djangocms 模板才能正常引用这些文件.

        <link href="css/bootstrap.min.css" rel="stylesheet"> 
        ➜ <link href="{% static "css/bootstrap.min.css" %}" rel="stylesheet">

        <script src="js/jquery.js"></script>
        ➜ <script src="{% static "js/jquery.js" %}"></script>






🔵 模块.

    🔸 模块简介
        djangocms 提供很多非常有用的模块如: bootstrap3 和 news & blogs
        这些查看都可以在 DjangoCMS 官方市场里找到:    https://marketplace.django-cms.org/en/addons/

        每个模块有详细的安装教程.
        一般都是用 pip install xxx 来安装. 
        然后添加到 INSTALLED_APPS 中.
        然后重新生成数据表.
            python manage.py makemigrations
            python manage.py migrate

        这些插件其实就是 别人写好的模块. 和你自己新建模块一样的用法.



        ❗️ 每一页都可以单独设置的!! 在 page 里面.   可以选择单独模板,...等等!!!
        一个模板好像只能用在一个页面上..










    🔸 插件   Aldryn Forms
        https://github.com/aldryn/aldryn-forms


        ⦿ 作用 ? 创建表格的???

        ⦿ 安装: pip install aldryn-forms

        ⦿ 配置 indtalled_apps
            …
            'absolute',
            'aldryn_forms',
            'aldryn_forms.contrib.email_notifications',
            'captcha',
            'emailit',
            'filer',
            …

        ⦿ 更新数据库  python manage.py migrate

        ⦿ 使用


    🔸 djangocms-blog
        一个博客模块.
        官网: https://www.nephila.it/it/blog/2014/02/07/blogging-django-cms-djangocms-blog/
        Github:  https://github.com/nephila/djangocms-blog
        安装手册: http://djangocms-blog.readthedocs.io/en/latest/installation.html


        ⦿ pip install djangocms-blog

        ⦿ INSTALLED_APPS = [
            ...
            'filer',
            'easy_thumbnails',
            'aldryn_apphooks_config',
            'cmsplugin_filer_image',
            'parler',
            'taggit',
            'taggit_autosuggest',
            'meta',
            'djangocms_blog',
            ...
            ]

        ⦿ ./manage.py cms check

        ⦿ python manage.py migrate








⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  项目迁移 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️------⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️⬛️
GCE 肯定的污蔑我挖矿.. 把我项目关了. 两天也不见回复.  我的数据怎么办啊!!!
数据库是做了主从的. 但是 djangocms项目没有啊..
看来这货也需要用mysql. 把最重要的数据库进行备份了....



