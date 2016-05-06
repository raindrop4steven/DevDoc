## 七牛存储

### 1. 七牛存储批量上传实现

1. [qiniu_upload](https://github.com/huhuanming/qiniu_upload)
2. [NSOperationQueue](http://blog.cnbluebox.com/blog/2014/07/01/cocoashen-ru-xue-xi-nsoperationqueuehe-nsoperationyuan-li-he-shi-yong/)
3. [编程模型介绍](http://developer.qiniu.com/article/developer/programming-model.html)
4. [七牛回调接口](http://developer.qiniu.com/article/kodo/kodo-developer/up/response-types.html#callback)

### 2. memo
1. 自定义变量:必须以 x: 开头，不限个数。里面的内容将在 callbackBody 参数中的 $(x:custom_field_name) 求值时使用
2. [生成上传Token](http://developer.qiniu.com/article/developer/security/upload-token.html)
3. 业务服务器生成综合上传策略生成上传凭证，客户端使用上传凭证进行Qiniu上传，由Qiniu向业务服务器提出回掉。
    - 将user_token加在上传策略中返回给用户，然后加入Qiniu回调的header中，即可进行判断。
    - Authorization:QBox iN7NgwM31j4-BZacMjPrOQBs34UG1maYCAQmhdCV:tDK-3f5xF3SJYEAwsll5g=
#### 服务器架构
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