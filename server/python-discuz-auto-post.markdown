### Discuz 自动登陆回帖发帖

    #-*-coding:utf-8-*-
    import urllib2, urllib, cookielib
    import re
    import getpass
    import sqlite3
    import random
    import time
     
    class Discuz:
        def __init__(self,user,pwd,args):
            self.username = user
            self.password = pwd
            self.args = args
            self.regex = {
                'loginreg':'',
                'replyreg':'',
                'tidreg': '[\s\S]+?'
            }
            self.conn = None
            self.cur = None
            self.islogin = False
            self.login()
            self.InitDB()
     
        def login(self):
            try:
                loginPage = urllib2.urlopen(self.args['loginurl']).read()
                formhash = re.search(self.regex['loginreg'], loginPage)
                formhash = formhash.group(1)
                #print 'login formhash:', formhash
                print 'start login...'
                cj = cookielib.CookieJar()
                opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cj))
                user_agent = 'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; Mozilla/4.0 \
                        (compatible; MSIE 6.0; Windows NT 5.1; SV1) ; .NET CLR 2.0.507'
                opener.addheaders = [('User-agent', user_agent)]
                urllib2.install_opener(opener)
                logindata = urllib.urlencode({
                    'cookietime':    2592000,
                    'formhash': formhash,
                    'loginfield':'username',
                    'username':    self.username,
                    'password':    self.password,
                    'questionid':    0,
                    'referer': self.args['referer']
                    })
                request = urllib2.Request(self.args['loginsubmiturl'],logindata)
                response = urllib2.urlopen(request)
                self.islogin = True
                print 'login success...'
            except Exception,e:
                    print 'loggin error: %s' % e
     
        def PostReply(self, fid, tid, content):
            try:
                sql = "select * from post where fid='%s' and tid='%s'" % (fid,tid)
                self.cur.execute(sql)
                if self.cur.rowcount == -1:
                    tidurl = self.args['tidurl'] % tid
                    replysubmiturl = self.args['replysubmiturl'] % (fid,tid)
                    tidPage = urllib2.urlopen(tidurl).read()
                    formhash = re.search(self.regex['replyreg'], tidPage)
                    formhash = formhash.group(1)
                    #print 'reply formhash:', formhash
                    print 'start reply...'
                    replydata = urllib.urlencode({
                        'formhash': formhash,
                        'message': content,
                        'subject': '',
                        'usesig':'1'
                    })
                    request = urllib2.Request(replysubmiturl,replydata)
                    response = urllib2.urlopen(request)
                    sql = "insert into post values ('%s', '%s', '%d')" % (fid, tid, 1)
                    self.cur.execute(sql)
                    self.conn.commit()
                    print 'reply success for [%s]' % tidurl
                else:
                    print 'Skip! Thread:%s is already replied...' % tid
            except Exception, e:
                    print 'reply error: %s' % e
     
        def GetTids(self, fid):
            if self.islogin:
                fidurl = self.args['fidurl'] % fid
                response = urllib2.urlopen(fidurl)
                content = response.read()
                tids = re.findall(self.regex['tidreg'], content)
                return tids
            else:
                print 'Error Please Login...'
     
        def InitDB(self):
            self.conn = sqlite3.connect('data.db')
            self.cur = self.conn.cursor()
            sql = '''create table if not exists post (
                fid text,
                tid text,
                replied integer)'''
            self.cur.execute(sql)
            self.conn.commit()
     
    if __name__ == '__main__':
        username = raw_input('username:').strip()
        password = getpass.getpass('password:').strip()
        args = {
                'loginurl': 'http://www.xxx.com/logging.php?action=login',
                'loginsubmiturl': 'http://www.xxx.com/logging.php?action=login&amp;loginsubmit=yes',
                'fidurl': 'http://www.xxx.com/forum-%s-1.html',
                'tidurl': 'http://www.xxx.com/thread-%s-1-1.html',
                'replysubmiturl': 'http://www.xxx.com/post.php?action=reply&amp;replysubmit=yes&amp;infloat=yes&amp;handlekey=fastpost&amp;fid=%s&amp;tid=%s',
                'referer':'http://www.xxx.com/index.php'
        }
        dz = Discuz(username, password,args)
        fid = '45'
        tids = dz.GetTids('45')
        replylist = [
                u'不错，支持一下，呵呵',
                u'已阅，顶一下',
                u'看看，顶你，呵呵',
                u'多谢分享，顶一下',
                u'说的不错，支持一下',
                u'提着水桶到处转，哪里缺水哪里灌！ ',
                u'你太油菜了!'
        ]
        for tid in tids:
            content = random.choice(replylist)
            content = content.encode('gbk')
            dz.PostReply('45',tid, content)
            time.sleep(20)