## Nginx Upload Module

### 一、编译安装Nginx
为了使用Nginx Upload Module，需要编译安装Nginx，将upload module编译进去。upload module的代码可以去Github上下载： Upload Module

之后的编译安装Nginx这里就不介绍，不了解的可以参考： [Ubuntu 14.10下源码编译安装Nginx 1.8.0](http://www.bkjia.com/Linux/1002916.html)

### 二、Nginx配置
Nginx upload module的简单配置如下：


    server {
        listen *:80 default_server;
        server_name 192.168.1.251;
        client_max_body_size 20m;
        client_body_buffer_size 512k;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE_ADD $remote_addr;
        location /upload {
        # 转到后台处理URL,表示Nginx接收完上传的文件后，然后交给后端处理的地址
        upload_pass @python;
        # 临时保存路径, 可以使用散列
        # 上传模块接收到的文件临时存放的路径， 1 表示方式，该方式是需要在/tmp/nginx_upload下创建以0到9为目录名称的目录，上传时候会进行一个散列处理。
        upload_store /tmp/nginx_upload;
        # 上传文件的权限，rw表示读写 r只读
        upload_store_access user:rw group:rw all:rw;
        set $upload_field_name "file";
        # upload_resumable on;
        # 这里写入http报头，pass到后台页面后能获取这里set的报头字段
        upload_set_form_field "${upload_field_name}_name" $upload_file_name;
        upload_set_form_field "${upload_field_name}_content_type" $upload_content_type;
        upload_set_form_field "${upload_field_name}_path" $upload_tmp_path;
        # Upload模块自动生成的一些信息，如文件大小与文件md5值
        upload_aggregate_form_field "${upload_field_name}_md5" $upload_file_md5;
        upload_aggregate_form_field "${upload_field_name}_size" $upload_file_size;
        # 允许的字段，允许全部可以 "^.*$"
        upload_pass_form_field "^.*$";
        # upload_pass_form_field "^submit$|^description$";
        # 每秒字节速度控制，0表示不受控制，默认0, 128K
        upload_limit_rate 0;
        # 如果pass页面是以下状态码，就删除此次上传的临时文件
        upload_cleanup 400 404 499 500-505;
        # 打开开关，意思就是把前端脚本请求的参数会传给后端的脚本语言，比如：http://192.168.1.251:9000/upload/?k=23,后台可以通过POST['k']来访问。
        upload_pass_args on; 
        }
        location @python {
        proxy_pass http://localhost:9999;
        # return 200;  # 如果不需要后端程序处理，直接返回200即可
        }
    }
### 三、后端处理程序
这里我们使用Django作为后端处理程序，比较简单，具体如下：

首先创建Django项目：

django-admin.py startproject uploadmodule
然后，创建views.py文件，代码如下：

    # -*- coding: utf-8 -*-
    import os
    import json
    from django.http import HttpResponse
    from django.views.decorators.csrf import csrf_exempt
    UPLOAD_FILE_PATH = '/tmp/nginx_upload/'
    @csrf_exempt
    def upload(request):
        request_params = request.POST
        file_name = request_params['file_name']
        file_content_type = request_params['file_content_type']
        file_md5 = request_params['file_md5']
        file_path = request_params['file_path']
        file_size = request_params['file_size']
        ip_address = request.META.get('HTTP_X_REAL_IP') or request.META.get('HTTP_REMOTE_ADD')
        # save file to tmp
        new_file_name = '%s_%s' % (file_md5, ip_address)
        new_file_path = ''.join([UPLOAD_FILE_PATH, new_file_name, os.path.splitext(file_name)[-1]])
        with open(new_file_path, 'a') as new_file:
            with open(file_path, 'rb') as f:
                new_file.write(f.read())
        content = json.dumps({
            'name': file_name,
            'content_type': file_content_type,
            'md5': file_md5,
            'path': file_path,
            'size': file_size,
            'ip': ip_address,
        })
        response = HttpResponse(content, content_type='application/json; charset=utf-8')
        return response
    
### 四、示例
上面的代码完成之后，我们通过下面的命令启动Django后端程序：

cd uploadmodule/
python manage.py runserver 0.0.0.0:9999
然后，模拟POST请求：http://192.168.1.251/upload/，上传一个jpg文件，返回结果如下：

    {
        "name": "6125444419718417450.jpg",
        "ip": "192.168.1.121",
        "content_type": "image/jpeg",
        "path": "/tmp/nginx_upload/0000000002",
        "md5": "c3b1bd2e72694a8d5fc4548b9ecd9e18",
        "size": "37980"
    }

### 参考：

[Ubuntu 14.10下源码编译安装Nginx 1.8.0](http://www.bkjia.com/Linux/1002916.html)

[HttpUploadModule - Nginx Community](http://wiki.nginx.org/HttpUploadModule)

[vkholodkov/nginx-upload-module at 2.2](https://github.com/vkholodkov/nginx-upload-module/tree/2.2)

[Uploading to nginx using the nginx upload module with php_handler](https://github.com/blueimp/jQuery-File-Upload/wiki/Uploading-to-nginx-using-the-nginx-upload-module-with-php_handler)

### Link

[使用 Nginx Upload Module 实现上传文件功能](http://www.open-open.com/lib/view/open1435198673794.html)