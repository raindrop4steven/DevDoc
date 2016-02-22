## iOS UI misc

1. UITextField round cordor
	
		//first, you
		#import <QuartzCore/QuartzCore.h>
		
		//.....
		
		//Here I add a UITextView in code, it will work if it's added in IB too
		UITextView *textView = [[UITextView alloc] initWithFrame:CGRectMake(50, 220, 200, 100)];
		
		//To make the border look very close to a UITextField
		[textView.layer setBorderColor:[[[UIColor grayColor] colorWithAlphaComponent:0.5] CGColor]];
		[textView.layer setBorderWidth:2.0];
		
		//The rounded corner part, where you specify your view's corner radius:
		textView.layer.cornerRadius = 5;
		textView.clipsToBounds = YES;