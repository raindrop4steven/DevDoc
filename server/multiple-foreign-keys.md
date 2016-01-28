###[[AF] Flask SqlAlchemy Two Foreign Keys Referencing the Same Table (self.flask)](https://www.reddit.com/r/flask/comments/2o4ejl/af_flask_sqlalchemy_two_foreign_keys_referencing/)

I have the following SqlAlchemy Models for a small application I am working on. The problem I have is that the Payment model has two foreign keys to the User model. I am aware that the way I am doing the relationships in User to Payment is currently incorrect because SqlAlchemy has no way of knowing which foreign key to reference, but I'm not sure what the correct way to do it is. What I currently have in the User model was simply my best guess. If anyone can tell me the proper way to make these relationships of this kind I would greatly appreciate it.

	class User(db.Model):
	    __tablename__ = 'User'
	    ID = db.Column(db.Integer, primary_key=True)
	    FirstName = db.Column(db.String(64))
	    LastName = db.Column(db.String(64))
	    Email = db.Column(db.String(64), unique=True)
	    PwdHash = db.Column(db.String(100))
	    Payments = db.relationship("Payment", backref="Payer", lazy = "dynamic")
	    Received = db.relationship("Payment", backref="Receiver", lazy = "dynamic")
	
	class Payment(db.Model):
	    __tablename__ = "Payment"
	    id = db.Column(db.Integer, primary_key = True)
	    uidPayer = db.Column(db.Integer, db.ForeignKey("User.ID"))
	    uidReceiver = db.Column(db.Integer, db.ForeignKey("User.ID"))
	    amount = db.Column(db.Float)

### [Fix] 

Try This :

	Payments = db.relationship('Payment', backref = 'payer', lazy = 'dynamic', foreign_keys = 'Payment.uidPayer')
	Received = db.realtionship('Payment', backref = 'Receiver', lazy = 'dynamic, foreign_keys = 'Payment.uidReceiver')
