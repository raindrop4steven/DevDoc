# relayout collectionview

- (void)viewDidLayoutSubviews {
    [super viewDidLayoutSubviews];
    
    if(self.isFirstLayout) {
        self.isFirstLayout = NO;
        
        [self.musiCollectionView layoutIfNeeded];
        
        CGFloat height =  self.musiCollectionView.bounds.size.height * 0.5;
        
        [self.flowLayout setItemSize:CGSizeMake(height - 30 , height)];
    }
}