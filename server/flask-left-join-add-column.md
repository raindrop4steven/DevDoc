## Flask Sqlalchemy Left Join

### 1. Common Join
	-- statement --
	db.session.query(Voice, Favor.voice_id).outerjoin(Favor).all()

	-- result --
	[(<application.models.voice.Voice object at 0x7f529c150590>, 1), (<application.models.voice.Voice object at 0x7f529c150910>, 2), (<application.models.voice.Voice object at 0x7f529c150790>, None)]



### 2. Join subquery

1. First select from Favor and get all the subquery filter by (user_id = g.user.id)
2. Join

		-- First select from Favor and get all user's favor records --
		subq = db.session.query(Favor).filter(Favor.user_id == 1).subquery()
		-- Second outjoin with subquery, and c means column --
		db.session.query(Voice, subq.c.voice_id).outerjoin(subq).all()

		-- result --
		[(<application.models.voice.Voice object at 0x7f529c11f190>, None), (<application.models.voice.Voice object at 0x7f529c11f210>, 1), (<application.models.voice.Voice object at 0x7f529c11f3d0>, 2)]


### 3. Import orders

	from manager import *
	from application.models import *
	from application.models.user import *