## Source

1. [PageMenu](https://github.com/HighBay/PageMenu)
1. [Tip View](https://github.com/teodorpatras/EasyTipView)
1. [IGLDropDownMenu](https://github.com/bestwnh/IGLDropDownMenu)
1. [Blog ios big source](http://cnbin.github.io/blog/2015/09/16/ios-zi-liao-da-quan/)
1. [Objective Zip](https://github.com/gianlucabertani/Objective-Zip)
2. [Drop Notifications](https://github.com/terryworona/TWMessageBarManager)
2. [美拍架构](http://h2ex.com/713)
3. [文字效果](https://github.com/lexrus/LTMorphingLabel)
4. [Detail design](https://soyep.com/)

## FFMPEG
http://askubuntu.com/questions/35457/converting-aac-to-mp3-via-command-line

## SwipeView last item bug fix

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