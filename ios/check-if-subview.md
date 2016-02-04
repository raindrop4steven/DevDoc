## Check whether a view is subview of another view

You are probably looking for UIView's `-(BOOL)isDescendantOfView:(UIView *)view`; taken in UIView class reference.

Return Value `YES` if the `receiver is an immediate or distant subview of view` or if `view is the receiver itself`; otherwise NO.
You will end up with a code like :

	Objective-C
	- (IBAction)showPopup:(id)sender {
	    if(![self.myView isDescendantOfView:self.view]) { 
	        [self.view addSubview:self.myView];
	    } else {
	        [self.myView removeFromSuperview];
	    }
	}
	Swift
	@IBAction func showPopup(sender: AnyObject) {
	    if !self.myView.isDescendantOfView(self.view) {
	        self.view.addSubview(self.myView)
	    } else {
	        self.myView.removeFromSuperview()
	    }
	}