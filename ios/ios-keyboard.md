### iOS Keyboard Type and resign

1. Add button as super view of all elements. Or Editor->Arrange->Move Back/front...

2. Add its click event

3. Keyboard return type.
	
		[textField setReturnKeyType:UIReturnKeyDone];
	for dismissing the keyboard implement the <UITextFieldDelegate> protocol in your class, set
	
		textfield.delegate = self;
	and use
	
		- (void)textFieldDidEndEditing:(UITextField *)textField {
		    [textField resignFirstResponder];
		}
	or
	
		- (BOOL)textFieldShouldReturn:(UITextField *)textField {
		    [textField resignFirstResponder];
		    return YES;
		}