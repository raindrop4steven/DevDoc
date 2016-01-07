### Assign integer to NSString param

It means that you are passing an NSNumber where the called code expects an NSString or some other object that has a length method. You can tell Xcode to break on exceptions so that you see where exactly the length method gets called.

	hot.score = 0;
	[myLabel setText:hot.score];
	// This will cause error and hard to caught
	// Fix:
	// Transfer string from server