### Douban Audio Player
https://github.com/douban/DOUAudioStreamer


### TODO
- [N] HeaderView need datasource.
    - 读书，旅行，爱情物语，寂寞孤单
    - P图：书籍封面，旅行封面
- [N] Classcification of voice, like 9 diary and [fm](http://fm.xinli001.com/99388908)
    - Emotions: 开心, 悲伤，愤怒
    - 场景：睡前，旅行，失恋，一个人
- [N] Stretch Header View
    - [Stretch Header View](http://blog.matthewcheok.com/design-teardown-stretchy-headers/)

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



### Server
- Trigger after creating a user, a default album should be created.

        update_task_state = DDL('''\
        CREATE TRIGGER update_task_state UPDATE OF state ON obs
          BEGIN
            UPDATE task SET state = 2 WHERE (obs_id = old.id) and (new.state = 2);
          END;''')
        event.listen(Obs.__table__, 'after_create', update_task_state)
        
### Features
- [StreamingKit](https://github.com/tumtumtum/StreamingKit)

### Links
- [Sticky header](https://github.com/jamztang/CSStickyHeaderFlowLayout)
### Templates
- [mini site](http://minimalexhibit.com/)