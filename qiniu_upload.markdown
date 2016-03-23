## 七牛存储批量上传实现

1. [qiniu_upload](https://github.com/huhuanming/qiniu_upload)
2. [NSOperationQueue](http://blog.cnbluebox.com/blog/2014/07/01/cocoashen-ru-xue-xi-nsoperationqueuehe-nsoperationyuan-li-he-shi-yong/)
3. [编程模型介绍](http://developer.qiniu.com/article/developer/programming-model.html)
4. [七牛回调接口](http://developer.qiniu.com/article/kodo/kodo-developer/up/response-types.html#callback)


## 服务器架构
1. 构成
    1. Bussiness Server (aliyun)
        - Database
        - Token
    2. Storage Server (qiniu)
        - Mixed voice
        - Images
    3. Chat Server (RC)
        - Chat
        - Contact