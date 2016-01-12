### uicollectionView realod时机

This is caused by cells being added during layoutSubviews not at reloadData. Since layoutSubviews is performed during next run loop pass after reloadData your cells are empty. Try doing this:

	[self.collectionView reloadData];
	[self.collectionView layoutIfNeeded];
	[self configure cells]; 
I had similar issue and resolved it this way.