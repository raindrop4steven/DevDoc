## Test if static files served with NGINX

### 1. In your application setup code:
    @app.after_request
    def add_served_by_flask_header(response):
        response.headers["X-Served-By-Flask"] = "true"
        return response
        
### 2. And in your nginx configuration:

    location /static {
        alias /var/www/mySite/static;
        add_header X-Served-By-NGINX true always;
    }