### flask-sqlalchemy-uniqueConstraint

类定义
	
	class Favor(db.Model):
	    __tablename__ = 'favors'
	    id = db.Column(db.Integer, primary_key=True)
	    user_id = db.Column(db.Integer, db.ForeignKey('users.id'))
	    voice_id = db.Column(db.Integer, db.ForeignKey('voice.id'))
	    __table_args__ = (db.UniqueConstraint('user_id', 'voice_id', name='user_favor_voice_uc'),)

**注意**：__table_args__ = (db.UniqueConstraint(xxx),)