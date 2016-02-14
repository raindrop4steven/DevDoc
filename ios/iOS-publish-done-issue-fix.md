### iOS issue


#### 1. publish done issue

When pop up motion choice and after publish success, it returned to login.

#### Fix solution

1. HotViewController/NearViewController register **RDPPublishSuccessNotification**
1. Make pop motion with mode `presentViewController`, then push it
2. when publish done, post notification and then hot/near should reloadData


#### 2. Logout

1. When logout, post logout notification, update ProfileView

#### 3. Profile content

1. Add **Contacts**, **Notification**

#### 4. DetailViewController

1. Add `...` share menu, prove your voice


#### 5. Login

1. Profile's login page: like DuiTang's
2. Before publish + : like PianKe's

#### 6. Music download

1. When download success, push notification to localMusicViewController, it then reload data.


#### 7. Picture
1. When click show menu : Camera/Photo/preLoaded images.


#### POP

-----------------------------------------------------------------------------------------

TODO:
- 表情缩减到3个，效果和片刻一样，选择后立即跳转并隐藏，不在后续的之中。