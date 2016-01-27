### Flask-Migrate TroubleShoting

1. **Error: Target database is not up to date**

>After creating a migration, either manually or as --autogenerate, you must apply it with alembic upgrade head. If you used db.create_all() from a shell, you can use `alembic stamp head` to indicate that the current state of the database represents the application of all migrations.

	python manger.py db stamp head

2. Flask-Migrate 会在数据库中插入一张表：alembic_version,如果需要reset migrations需要将这张表清空或删除

3. Insert nullable=False column will get error with inital insert, you could break it into two steps.

	1. Add column without nullalbe=False.
	
			avator = db.Column(db.String(32))
	2. After upgrade succeed, you insert data with it then you could add nullable=False
	
			avator = db.Column(db.String(32), nullable=False)