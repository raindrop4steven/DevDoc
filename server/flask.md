### Flask param safe

To access parameters submitted in the URL (?key=value) you can use the args attribute:

	searchword = request.args.get('key', '')

### Exception Handler
[Custom Exception Handler](http://flask.pocoo.org/docs/0.10/patterns/apierrors/)