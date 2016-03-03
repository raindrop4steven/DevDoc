## Flask redirect

1. `from flask import redirect`

        return redirct('/static/data/album/girl.jpg', code=200)
    

2. request.form vs request.args

        "Mr.Liang: I've only used flask a bit but basically you can get POST data using
    
        myvar =  request.form["myvar"]
        and GET using
        
        myvar = request.args.get("myvar")"

3. flask string to datetime

        from datetime import datetimedate_object = datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')
    