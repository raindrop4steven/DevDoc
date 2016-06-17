作者：WiNrOOt
链接：https://www.zhihu.com/question/19657462/answer/13835748
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

1.  家喻户晓的域名：
怎么家喻户晓那是你的事情，
购买途径推荐http://www.godaddy.com。为什么？价格便宜，还有NameServer可以免费设置，不像DreamMonster虚拟主机一到期，连Nameserver都不给用了，当时我们http://sukikits.com就在这个上面郁闷了一下。而且支持支付宝
具体的注册过程可以baidu搜索“godaddy注册”。
2.  空间是根本：
    1.  最好是国内的。
    2.  空间不需要太大，几百兆就够了，你的产品网站没那么多内容。
    3.  建议购买Apache为WebServer的空间，后面做页面静态化需要。
    4.  尽可能挑一些口碑好的空间提供商。因为你在购买前给你演示速度的网站，跟你拿到手的时候速度会有天壤之别。
3.  域名备案
没有这个号码，你的网站将没有任何竞争力，不论你的产品有多么的优秀。在这片神奇的土地上你必须做的事情。当然空间商会帮你提交，你只需要填写资料，准备好电子版的照片，一定要填写能联系到你的手机，通管局会给你打电话确认信息的。
4.  伟大的DNSpod

    为什么要DNSpod
    用下来就是快，最初我做过对比，比我的的域名服务上提供的解析速度快。
    最重要的是在快的前提下，而且免费。


    设置比较简单，文档见https://www.dnspod.cn/Support

5.  名片上的企业邮箱

    弄好DNSpod以后我们就可以设置企业邮箱了。国内企业邮箱建议选择腾讯http://exmail.qq.com国外服务的话选择Gmail的吧。
    我们选择的是腾讯的企业邮箱，比较方便，防止和墙发生不愉快的事情。


    注册完成后会有引导内容，按照引导进行设置就可以了。帮助页面http://service.exmail.qq.com/。
    这样你就可以有自己的域名后缀的邮箱了，比如我的邮箱leon@sukikits.com就用的是腾讯企业邮箱。

6.  主题不是问题

    如果你想开发强大的网站功能请忽略这里。
    下面介绍的是要建立一个产品介绍的网站。
    看你的需求，如果你想快速简单上手，wordpress就可以满足，如果你喜欢折腾那你去研究joomla或着Drupla。
    为什么选择Wordpress。
    快速. 简便. 基本满足一个产品介绍类网站的需求。各类插件. 模板丰富。
    还有一个最主要的原因：用的人多，遇到问题随便搜索就可以找到解决方案。
    那么下面开始说一下模板的选择。
    http://www.elegantthemes.com/
    http://themeforest.net/
    到这两个网站去挑选你喜欢的模板吧。版本的获得方法要么购买要么百度搜索。
    模板选择注意点：
    a. 选择一款发布时间稍长
    b. 下载或者购买量较多
    c. 契合你心目中网站风格的。
    这样的次序主要是对网站的兼容性. 性能. 以及你自己使用的便捷性都会有较好的保证。
    说白了用户体验为王！
    来看一下http://sukikits.com的后台


    主题建议先试用再购买。

7.  速度才是王道

    分为两部分：
    a. 图片访问加速
    第一步，找一个云存储推荐又拍云存储https://www.upyun.com/是按照访问流量计费的。说白了就是一个单独的图片CDN 在页面加载的时候不用访问你那“共享百兆”流量的空间。
    第二步，修改你DNSPOD，在里面增加一个CNAME 如http://image.sukikits.com。具体设置会有引导。帮助见http://www.upyun.com/intro/custom.php


    第三步，尽量将你内页或者主题内的图片连接使用你指向又拍云存储的连接。
    另外有一个又拍云的插件可以批量转化。
    下载地址：
    http://ihacklog.com/php/wordpress/plugins/hacklog-remote-attachment-upaiyun-version.html

    b. 页面静态化
    
    使用这个插件：WP Super Cache。
    下载地址：http://wordpress.org/extend/plugins/wp-super-cache/
    为什么要用：
    引用一下百度的搜索“WP Super Cache 是WordPress官方开发人员Donncha开发，是当前最高效也是最灵活的WordPress静态缓存插件。它把整个网页直接生成 HTML 文件，这样 Apache 就不用解析 PHP 脚本，通过使用这个插件，能使得你的WordPress博客将显著的提速。”
    设置帮助文档：http://ooxx.me/wp-super-cache.orz
    还有一个W3 Total Cache也可以试一下。哈哈


8.  高贵的CDN

    http://webluker.com太可爱了，让CDN这个高级货，飞入寻常百姓家。而且每个月30G的免费流量配额，基本上小站是够用了。
    CDN的定义见：http://baike.baidu.com/view/21895.htm
    简单说来就是把用户访问网站时需要的资源放到访问比较快的服务器上。
    简介“Webluker是一站式运维服务综合平台，为用户提供稳定，高效，灵活的服务。提供网站加速. 域名管理. DNS解析. 云主机. 服务器监控. 网站监控告警等功能。”


    帮助文档：http://blog.webluker.com/

9.  监控是个宝

    走到这一步基本上网站基础建设已经完毕。
    这里我们开始设定网站的运行状态监控。其实前面提到的DNSpod和webluker都带有服务器监控。大家可以使用，同时我这里推荐一款专门做网站监控的网站，他可能更加专业一些。
    推荐原因：免费. 好用
    监控宝：http://www.jiankongbao.com/
    
    
    
    帮助页面：http://www.jiankongbao.com/faq

10.  数据统计

    数据统计重要性我就不罗嗦了。
    在我朝，就得用本地的东西。所以选择百度统计：http://tongji.baidu.com/
    基本好用。Google的统计也是比较好用的。萝卜白菜各有所爱。
    数据关系我就不截图了。。。。
    这里有一些介绍：http://yingxiao.baidu.com/support/topic/3.html
    记住忽略百度推广那些人给你打的电话。你是一个小站，没钱最推广。