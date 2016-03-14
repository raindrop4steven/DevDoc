### Flask SqlAlchemy Trigger insert/update/delete

Look at SQLAlchemy's [Mapper Events](http://docs.sqlalchemy.org/en/rel_0_7/orm/events.html#mapper-events). You can bind a callback function to the *after_insert, after_update, and after_delete events*.

**Example**:

    from sqlalchemy import event
    
    def after_insert_listener(mapper, connection, target):
        # 'target' is the inserted object
        print(target.id_user)
    
    event.listen(User, 'after_insert', after_insert_listener)
    
**Trigger**:

    from sqlalchemy import event
    
    update_task_state = DDL('''\
    CREATE TRIGGER update_task_state UPDATE OF state ON obs
      BEGIN
        UPDATE task SET state = 2 WHERE (obs_id = old.id) and (new.state = 2);
      END;''')
    event.listen(Obs.__table__, 'after_create', update_task_state)
    
#### Connection text sql statements
[how-to-execute-raw-sql-in-sqlalchemy-flask-app](http://stackoverflow.com/questions/17972020/how-to-execute-raw-sql-in-sqlalchemy-flask-app)

    # Trigger to insert a default album after insert a new user
    @event.listens_for(User, 'after_insert')
    def after_insert_listener(mapper, connection, target):
        connection.execute(text('insert into %s (user_id, album_name) values(:user_id, :album_name)' % Album.__tablename__),{'user_id' : target.id, 'album_name': 'default album'})
        


#### Create if not exists

    from sqlalchemy import *
    
    """
    INSERT INTO example_table
        (id, name)
    SELECT 1, 'John'
    WHERE
        NOT EXISTS (
            SELECT id FROM example_table WHERE id = 1
        );
    """
    
    m = MetaData()
    
    example_table = Table("example_table", m,
                            Column('id', Integer),
                            Column('name', String)
                        )
    
    sel = select([literal("1"), literal("John")]).where(
               ~exists([example_table.c.id]).where(example_table.c.id == 1)
          )
    
    ins = example_table.insert().from_select(["id", "name"], sel)
    print(ins)
output:

    INSERT INTO example_table (id, name) SELECT :param_1 AS anon_1, :param_2 AS anon_2 
    WHERE NOT (EXISTS (SELECT example_table.id 
    FROM example_table 
    WHERE example_table.id = :id_1))
    


Solution 2

    INSERT INTO example_table
        (id, name)
    SELECT 1, 'John'
    WHERE
        NOT EXISTS (
            SELECT id FROM example_table WHERE id = 1
        );