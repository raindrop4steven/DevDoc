### 1. UINavigationController transparent

	Set "View controller-based status bar appearance” to NO in your info.list file;
	
	[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
	to -application:didFinishLaunchingWithOptions: of the AppDelegate.m.
	
	Note: UIStatusBarStyleDefault is the default value for the status bar style, it'll show black content instead. Both UIStatusBarStyleBlackTranslucent & UIStatusBarStyleBlackOpaque are deprecated in iOS 7.0.


### 2. UITextField Transparent

	- (void)setInputField:(UITextField *)textField placeHolder:(NSString *)placeHolder height:(CGFloat)height{
	    textField.layer.borderColor = [UIColor whiteColor].CGColor;
	    textField.layer.borderWidth = 0.8f;
	    textField.layer.cornerRadius = height/2;
	    textField.clipsToBounds = YES;
	    textField.backgroundColor = [UIColor clearColor];
	    NSAttributedString *userNamePlaceholder = [[NSAttributedString alloc] initWithString:placeHolder attributes:@{ NSForegroundColorAttributeName : [UIColor whiteColor] }];
	    textField.attributedPlaceholder = userNamePlaceholder;
	    textField.textColor = [UIColor whiteColor];
	}