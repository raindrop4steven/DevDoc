## Flask WTF tip

1. Get wtf form vaidation error

        def flash_errors(form):
        for field, errors in form.errors.items():
            for error in errors:
                flash(u"Error in the %s field - %s" % (
                    getattr(form, field).label.text,
                    error
                ))
                
2. http://flask.pocoo.org/docs/0.10/patterns/wtforms/