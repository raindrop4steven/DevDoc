## viewDidLayoutSubviews

self.view.size 600x600

    - (void)viewDidLoad {
        [super viewDidLoad];
        [self setupCollectionView];
    }
    
    - (void)viewDidLayoutSubviews {
        [super viewDidLayoutSubviews];
        NSLog(@"in view did layout subviews");
        [self.flowlayout setItemSize:self.collectionView.bounds.size];
    }