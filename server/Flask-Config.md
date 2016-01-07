#Flask config实践

Flask很赞的特点之一就是可扩展性强，非常灵活，对于config来说也是如此。Flask官方文档中已经提及了非常多的方法，以及一些有用的建议。我在Flask项目开发中的config实践，则是基于文档中提到的类继承方案，并通过环境变量来切换不同的config配置。

##需求
比较好的config方案是怎样的呢？我觉得有如下几点：

- 针对不同的环境分离配置（Development、Production等）
- 能够方便地在不同配置之间切换
- 适用于多人协作
- 易于维护
从这些需求点出发，下面分享一下我在Flask项目中的实践经验。

##项目结构
	/project_root
	    /config
	        __init__.py
	        default.py
	        development.py
	        development_sample.py
	        production.py
	        production_sample.py
	        testing.py
	    /project
	        __init__.py
	        ...
所有的配置文件都存放在`config`包中，而Flask app则位于`project`包。对config包中不同文件的做简单的说明：

- `__init__.py`：在这里定义加载配置的函数
- `default.py`：默认配置
- `development.py`（不签入Git）：用于开发的config，每个开发人员的development config可以自定义
- `development_sample.py`：development.py的模板，每当有新成员加入dev团队时，只需将其另存为development.py，然后根据自己的情况填充对应项即可
- `production.py`（不签入Git）：用于生产服务器的config，最好是由专人来管理production.py，其他dev需要在服务器增加config项，一律向此人申请
- `production_sample.py`：production.py的模板，填好后可以scp到服务器端
- `testing.py`：用于测试的配置，测试的config应该是环境无关的（比如一般会使用sqlite数据库进行测试，这样就无需配置账号密码了），所以需要签入Git中
##config类继承结构
采用了基于类继承的config结构，保存默认配置的Config类作为基类，其他类继承之，如下：

	# default.py
	class Config(object):
	    ...

	# development.py
	class DevelopmentConfig(Config)
	    ...

	# production.py
	class ProductionConfig(Config)
	    ...

	# testing.py
	class TestingConfig(Config)
	    ...
这样做的好处首先在于

1. 通过继承达到了config复用的目的。
2. 第二个好处来自IDE，比如PyCharm可以对类中属性是否为override进行提示，如下图：



有圈圈+向上箭头标志的行就是override自父类，没有的就是自己定义的啦，一目了然。

## 加载策略
之前提到过，在·config/__init__.py·中会定义用于加载config的函数，加载策略如下：

1. 读取环境变量MODE，根据MODE的取值加载不同的config
2. 若MODE环境变量不存在（或不合法），则默认加载development config
3. 若development config无法导入，则使用default config  
####用代码表达就是：

	# coding: UTF-8
	import os
	
	def load_config():
	    """加载配置类"""
	    mode = os.environ.get('MODE')
	    try:
	        if mode == 'PRODUCTION':
	            from .production import ProductionConfig
	            return ProductionConfig
	        elif mode == 'TESTING':
	            from .testing import TestingConfig
	            return TestingConfig
	        else:
	            from .development import DevelopmentConfig
	            return DevelopmentConfig
	    except ImportError, e:
	        from .default import Config
	        return Config
##加载config
在定义好config结构之后，就可以加载了。需要尽早加载config，以便flask的一些第三方插件能够读取配置，比如Flask-SQLAlchemy：

	from flask import Flask
	from config import load_config  # 绝对导入
	from .models import db
	
	def create_app():
	    """创建Flask app"""
	    app = Flask(__name__)
	
	    # Load config
	    config = load_config()
	    app.config.from_object(config)
	
	    db.init_app(app)
	    ...
##使用config
	from flask import current_app
	
	config = current_app.config
	SITE_DOMAIN = config.get('SITE_DOMAIN')
##切换config
通过改变环境变量MODE来切换config，在不同应用场景下有不同的方法：

1. 可以在代码中直接改变环境变量：

		import os
		
		os.environ['MODE'] = 'TESTING'
2. 在使用Fabric部署时，可以通过shell_env设置环境变量

		from fabric.api import shell_env
		
		with shell_env(MODE='PRODUCTION'):
    	# do something
3. 在使用Supervisor管理Gunicorn进程时，可以通过environment配置项来设置环境变量：

		[program:project_name]
		command = /var/www/project/venv/bin/gunicorn -c deploy/gunicorn.conf wsgi:app
		directory = /var/www/project
		user = deploy
		autostart = true
		autorestart = true
		environment = MODE="PRODUCTION"
个人建议在生产服务器上默认启用PRODUCTION模式，可以在/etc/profile末尾添加一行：

		export MODE=PRODUCTION
我的一个开源项目[Flask-Boost](https://github.com/hustlzp/Flask-Boost)已经用上了这种config策略，感兴趣的童鞋可以去看看。

## 相关链接
[牛人博客](http://www.hustlzp.com/)