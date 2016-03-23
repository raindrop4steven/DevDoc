## Source

1. [PageMenu](https://github.com/HighBay/PageMenu)
1. [Tip View](https://github.com/teodorpatras/EasyTipView)
1. [Blog ios big source](http://cnbin.github.io/blog/2015/09/16/ios-zi-liao-da-quan/)
1. [Objective Zip](https://github.com/gianlucabertani/Objective-Zip)
2. [Drop Notifications](https://github.com/terryworona/TWMessageBarManager)
4. [done] [Detail design](https://soyep.com/) 
5. [雷达刷新](https://www.behance.net/gallery/35042409/Vispo-Social-Network)
6. [instalgram profile & snapshot](http://www.vinfotech.com/solutions/social-network-design)
7. [nearby](https://www.behance.net/gallery/33511487/TREND-Free-UI-KIT)
8. [well, foody and nice looking](https://www.behance.net/gallery/34101486/Riverr-App-Concept)
## Link
1. [美拍架构](http://h2ex.com/713)
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