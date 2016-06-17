## 坑

1. [NSUserdefaults boolForKey vs objectForKey]
2. storyboard中可以通过修改xml，将button放到最后


## 学到
设置textField光标颜色：textField.tintColor


## 在parentController里面这样写，指定子Controller的返回标题。
self.navigationItem.backBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"Back" style:UIBarButtonItemStylePlain target:nil action:nil];

## 保持屏幕常亮
Add this line of code on your application delegate:

    [[UIApplication sharedApplication] setIdleTimerDisabled:YES];