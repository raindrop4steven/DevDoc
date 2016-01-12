### Flask Deploy

- [flask + tornado + nginx + supervisord](https://gist.github.com/ewheeler/1262989)
- [Deploy a Flask Application on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- [nginx flask](http://www.oschina.net/translate/serving-flask-with-nginx-on-ubuntu)


Since you managed to install it anyhow first thing you've to do is to remove it completely with the configuration files

Follow these steps to remove it completely and install it again.

Open terminal and execute these commands:

sudo apt-get autoremove nginx
sudo apt-get --purge remove nginx
sudo apt-get autoremove && sudo apt-get autoclean
sudo find / | grep nginx | sudo xargs rm -rf
the last command will remove the repository also so you've to add it again by:

sudo add-apt-repository ppa:nginx/stable
Now try to install it again by:

sudo apt-get update && sudo apt-get -f install nginx
Hope it would solve your issue. Reply if you get any error at any particular command describing the command.

This is the output of

sudo dpkg -l | grep nginx:

ii  nginx                                       1.4.3-1~precise0                                    small, powerful, scalable web/proxy server
ii  nginx-common                                1.4.3-1~precise0                                    small, powerful, scalable web/proxy server - common files
ii  nginx-full                                  1.4.3-1~precise0                                    nginx web/proxy server (standard version)
whereis nginx:

nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx /usr/share/man/man1/nginx.1.gz