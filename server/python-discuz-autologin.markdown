### Python Discuz auto login

http://blogread.cn/it/article/2170

    def Login(user,pwd): 
        loginpage = urllib.urlopen('http://bbs.xxx.com/logging.php?action=login').read() 
        login_soup = BeautifulSoup(loginpage) 
        formhash_tag = login_soup.find('input',attrs={'name':'formhash'}) 
        formhash = formhash_tag['value'] 
         
        params = { 
                "answer":"", 
                "formhash":formhash, 
                "loginfield":"username", 
                "loginsubmit":"", 
                "password":pwd, 
                "questionid":"0", 
                "referer":"index.php", 
                "username":user, 
                } 
        jar = cookielib.CookieJar() 
        handler = urllib2.HTTPCookieProcessor(jar) 
        opener = urllib2.build_opener(handler) 
        urllib2.install_opener(opener) 
        user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)' 
        headers = { 'User-Agent' : user_agent } 
        req = urllib2.Request(login_url) 
        req.add_header('User-Agent',user_agent) 
        enparams = urllib.urlencode(params) 
        page = urllib2.urlopen(req,enparams) 
        data = page.read() 
        global g_cookie 
        global g_formhash 
        g_cookie = page.info()['set-cookie'] 
        t_cookie = re.sub(r'poK_formhash=deleted','',g_cookie) 
        r_formhash = re.search(r"poK_formhash=[^;]+",t_cookie) 
        if r_formhash: 
            g_formhash = re.sub(r'poK_formhash=','',r_formhash.group()) 
        return