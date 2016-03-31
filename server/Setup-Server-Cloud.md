### Setup Server on Azure

#### 1. Install Packages

```bash
# Postgresql
sudo apt-get install postgresql postgresql-server-dev-9.3 postgresql-postgis-9.3

# Git
sudo apt-get install git

# Python
sudo apt-get install python-dev python-virtualenv

# Nginx
sudo apt-get install nginx
```

#### 2. Database Setup

```bash
# Add dbuser to postgres
sudo adduser dbuser

# Switch to user **postgres**
sudo su - postgres

# Login postgresql as user postgres without password
psql

# Set password for it
\password postgres

# Create dbuser
CREATE USER dbuser WITH PASSWORD 'xiaodaozong';
CREATE DATABASE raindrop OWNER dbuser;
GRANT ALL PRIVILEGES ON DATABASE raindrop TO dbuser;

# Connect to raindrop database

\c raindrop
# Enable Postgis as super user postgres
CREATE EXTENSION postgis;

# List installed extensions
\dx

# Relogin as dbuser
psql -U dbuser -d raindrop -h localhost -p 5432

```

#### 3. Deploy Application

```bash
# Create website folder
sudo mkdir -p /var/www
sudo chown -R steven:steven /var/www

# Git clone to solar
git clone https://github.com/raindrop4steven/Solar.git
# git checkout slim

# Virtualenv
cd /var/www
virtualenv venv && source venv/bin/activate
pip install -r requirements.txt

# Copy nginx config file to /etc/nginx/sites-*
# First remove default config in nginx/sites-*
sudo rm /etc/nginx/sites-available/default
sudo rm /etc/nginx/sites-enabled/default

sudo cp tornado.conf /etc/nginx/sites-available/
sudo cp tornado.conf /etc/nginx/sites-enabled/

# Change server_name in tornado.conf
server_name mcserver.chinacloudapp.cn

# Supervisord
# Create directory for supervisord log
sudo mkdir -p /var/log/supervisord && sudo chown -R steven:steven !$

# Folder deploy - create database tables
python manager.py createdb

# Start engine
supervisord -c supervisor.conf
supervisorctl -c supervisor.conf reload
supervisorctl -c supervisor.conf status
supervisorctl -c supervisor.conf start solar | all
supervisorctl -c supervisor.conf stop solar | all
```
