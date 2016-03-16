### Compile thrid module to nginx

1. git clone -b my-branch git@github.com:user/myproject.git
2. 
        apt-get install libpcre3 libpcre3-dev
        
        wget -P /tmp http://nginx.org/download/nginx-1.6.2.tar.gz
        tar -zxvf /tmp/nginx-1.6.2.tar.gz -C /tmp
        
        wget -P /tmp https://github.com/vkholodkov/nginx-upload-module/archive/2.2.0.tar.gz
        tar -zxvf /tmp/2.2.0.tar.gz -C /tmp
        
        cd /tmp/nginx-1.6.2
        
        ./configure --add-module=/tmp/nginx-upload-module-2.2.0
        make
        make install
        
3. site-enabled magic
    
        http {
            ...
            include /etc/nginx/sites-enabled/*;
        }
        

4. compile option

        ./configure --prefix=/usr/local/nginx --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --with-http_ssl_module --with-http_gzip_static_module --add-module=../nginx-upload-module

## Link

[Nginx upload module vs Flask](http://blog.thisisfeifan.com/2013/03/nginx-upload-module-vs-flask.html)
[Persmission](https://www.digitalocean.com/community/questions/how-do-i-set-permissions-for-uploading-media-in-wordpress-ubuntu-14-04-1-nginx-1-4-6)