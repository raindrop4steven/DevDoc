#Flask-Migrate: Alembic database migration wrapper for Flask

In this post I introduce you to Flask-Migrate, a new database migration handler for Flask based on Alembic that I just made public.

##Using Flask-Migrate
Flask-Migrate provides a set of command line options that attach to Flask-Script.

### 1. To install the extension you use pip as usual:

	$ pip install flask-migrate

As part of the installation you will also get Flask, Flask-SQLAlchemy and Flask-Script.

Below is a sample application that initializes Flask-Migrate and registers it with Flask-Script. As is typically the case with Flask-Script, the script is called manage.py:

	from flask import Flask
	from flask.ext.sqlalchemy import SQLAlchemy
	from flask.ext.script import Manager
	from flask.ext.migrate import Migrate, MigrateCommand
	
	app = Flask(__name__)
	app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
	
	db = SQLAlchemy(app)
	migrate = Migrate(app, db)
	
	manager = Manager(app)
	manager.add_command('db', MigrateCommand)
	
	class User(db.Model):
	    id = db.Column(db.Integer, primary_key = True)
	    name = db.Column(db.String(128))
	
	if __name__ == '__main__':
	    manager.run()

When you run the application you get an additional db option in the command line (you can call it differently if you want, of course):

	$ python manage.py --help
	usage: manage.py [-h] {shell,db,runserver} ...
	
	positional arguments:
	  {shell,db,runserver}
	    shell               Runs a Python shell inside Flask application context.
	    db                  Perform database migrations
	    runserver           Runs the Flask development server i.e. app.run()
	
	optional arguments:
	  -h, --help            show this help message and exit
	The db command exposes most of the Alembic options:
	
	$ python manage.py db --help
	usage: Perform database migrations
	
	positional arguments:
	  {upgrade,migrate,current,stamp,init,downgrade,history,revision}
	    upgrade             Upgrade to a later version
	    migrate             Alias for 'revision --autogenerate'
	    current             Display the current revision for each database.
	    stamp               'stamp' the revision table with the given revision;
	                        dont run any migrations
	    init                Generates a new migration
	    downgrade           Revert to a previous version
	    history             List changeset scripts in chronological order.
	    revision            Create a new revision file.
	
	optional arguments:
	  -h, --help            show this help message and exit
### 2. To add migration support to your database you just need to run the init command:

	$ python manage.py db init
	  Creating directory /home/miguel/app/migrations...done
	  Creating directory /home/miguel/app/migrations/versions...done
	  Generating /home/miguel/app/alembic.ini...done
	  Generating /home/miguel/app/migrations/env.py...done
	  Generating /home/miguel/app/migrations/env.pyc...done
	  Generating /home/miguel/app/migrations/README...done
	  Generating /home/miguel/app/migrations/script.py.mako...done
	  Please edit configuration/connection/logging settings in
	  '/home/miguel/app/migrations/alembic.ini' before proceeding.

Note that you should replace manage.py with the name of your launch script if you used a different name.

When you use Alembic alone you have to edit a couple of configuration files, but Flask-Migrate handles all that for you. When the init command completes you will have a migrations folder with the configuration files ready to be used.

### 3. To issue your first migration you can run the following command:

	$ python manage.py db migrate
	INFO  [alembic.migration] Context impl SQLiteImpl.
	INFO  [alembic.migration] Will assume non-transactional DDL.
	INFO  [alembic.autogenerate] Detected added table 'user'
	  Generating /home/miguel/app/migrations/versions/4708a5190f2_.py...done

The migrate command adds a new migration script. You should review it and edit it to be accurate, as Alembic cannot detect all changes that you make to your models. In particular it does not detect indexes, so those need to be added manually to the script.

### 4. If you prefer to write your migration scripts from scratch then use revision instead of migrate:

	$ python manage.py db revision
	  Generating /home/miguel/app/migrations/versions/15c04479d683_.py...done
	You can read Alembic's documentation to learn how to write migration scripts.

### 5. The next step is to apply the migration to the database. For this you use the upgrade command:

	$ python manage.py db upgrade
	INFO  [alembic.migration] Context impl SQLiteImpl.
	INFO  [alembic.migration] Will assume non-transactional DDL.
	INFO  [alembic.migration] Running upgrade None -> 4708a5190f2, empty message

And that's it! Your database is now synchronized with your models.

You should add all the files in the migrations folder to version control along with your source files. If you need to update another system to the latest database version you just need to update your source tree on that other system and then run db upgrade, like you did above.