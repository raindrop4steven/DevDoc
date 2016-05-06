### TODO

- [息提醒设置](http://www.rongcloud.cn/docs/ios.html#消息提醒设置)
- [Login Design](https://www.behance.net/gallery/34101486/Riverr-App-Concept)

### Done
- [Y] Query nickname and avatar in hot and near, easy for chat userinfo.[done]
- [Y] Voice, User need datatime column in DB.[done]
- [Y] Pause and record when recording.
    - record time < 5s, don't provide bg music mixing.
    - record time > 15s, provide music mixing.
- [Y] Check valid email format when login and register

        BOOL NSStringIsValidEmail(NSString *checkString)  
        {  
            NString *stricterFilterString = @"[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,4}";  
            NSString *laxString = @".+@.+\.[A-Za-z]{2}[A-Za-z]*";  
            NSString *emailRegex = stricterFilter ? stricterFilterString : laxString;  
            NSPredicate *emailTest = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", emailRegex];  
            return [emailTest evaluateWithObject:checkString];  
        }
        
- [Y] HeaderView need datasource.
    - 读书，旅行，爱情物语，寂寞孤单
    - P图：书籍封面，旅行封面

- [Y] https://github.com/terryworona/TWMessageBarManager

### Templates
- [mini site](http://minimalexhibit.com/)

### Bug

1. Remove last tailView and then added it back

        //enable loading view
         if (_loadingViewEnabled)
         {
             //remove loading view
             if (([self currentPage] < [self numberOfPages] - 1) && _tailLoadingView)
             {
                 [_tailLoadingView removeFromSuperview];
             }
             
             //add loading view
             if ([self currentPage] == [self numberOfPages] - 1)
             {
                 if (_tailLoadingView.superview == nil) {
                     
                     if (_tailLoadingView == nil) {
                         if (!_vertical) {
                             _tailLoadingView = [[TailLoadingView alloc] initWithFrame:CGRectMake(_scrollView.contentSize.width, 0,  80, _scrollView.bounds.size.height)];
                         }
                         else{
                             _tailLoadingView = [[TailLoadingView alloc] initWithFrame:CGRectMake(0, _scrollView.contentSize.height,  _scrollView.bounds.size.width, 60)];
                             [_tailLoadingView setIsVertical:_vertical];
                         }
                         _tailLoadingView.delegate = self;
                     }
                     
                     [_scrollView addSubview:_tailLoadingView];
                     [_scrollView bringSubviewToFront:_tailLoadingView];
                 }
             }
         }