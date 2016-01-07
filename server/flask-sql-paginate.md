### Flask SqlAlchemy Paginate
If you are using Flask-SqlAlchemy, see the paginate method of query. paginate offers several method to simplify pagination.

	record_query = Record.query.paginate(page, per_page, False)
	total = record_query.total
	record_items = record_query.items
**First page should be 1 otherwise the .total returns exception divided by zero**

[Stack OverFlow](http://stackoverflow.com/questions/9916094/sqlalchemy-and-paging)