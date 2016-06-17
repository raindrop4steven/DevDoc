### [Some solutions](http://stackoverflow.com/questions/15985574/uicollectionview-auto-scroll-to-cell-at-indexpath)

1. in **viewDidLoad**

	self.view.setNeedsLayout()
	self.view.layoutIfNeeded()

2. in **viewWillAppear**
	
	[self.view layoutIfNeeded];
	[self.collectionView scrollToItemAtIndexPath:indexPath atScrollPosition:UICollectionViewScrollPositionCenteredVertically animated:NO];


3. Well, this should be the correct answer:

    	- (void)viewDidLayoutSubviews {
    
    	// If we haven't done the initial scroll, do it once.
    	if (!self.initialScrollDone) {
    	    self.initialScrollDone = YES;
    
    		[self.collectionView scrollToItemAtIndexPath:self.myInitialIndexPath
    				                                    atScrollPosition:UICollectionViewScrollPositionRight animated:NO];
    		}
    	}