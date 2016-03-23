## Flask UnitTest

1. Send username:password header to self.client.get()?

        from base64 import b64encode
    
        headers = {
            'Authorization': 'Basic ' + b64encode("{0}:{1}".format(username, password))
        }
        
        rv = self.app.get('api/v1.0/{0}'.format(ios_sync_timestamp), headers=headers)
        
2. Import parent level package?

        import sys
        sys.path.append("/path/to/dir")
        from app import object
        

3. Get and Post format?

        self.app.post('/path-to-request', data=dict(var1='data1', var2='data2', ...))
        self.app.get('/path-to-request', query_string=params)