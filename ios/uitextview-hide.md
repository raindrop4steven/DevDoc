### [UITextView 退出键盘的方式](http://morizyun.github.io/blog/uitextview-done-close-keyboard-not-hide/)

	@implementation SampleViewController
	
	#pragma mark ビューが表示されるたびに呼ばれる
	-(void)viewDidAppear:(BOOL)animated {
	    [[NSNotificationCenter defaultCenter] removeObserver:self];
	    [super viewDidDisappear:animated];
	
	    // キーボード表示、非表示の際のイベントを設定
	    [[NSNotificationCenter defaultCenter] addObserver:self
	                                             selector:@selector(keyboardWasShown:)
	                                                 name:UIKeyboardDidShowNotification object:nil];
	    [[NSNotificationCenter defaultCenter] addObserver:self
	                                             selector:@selector(keyboardWasHidden:)
	                                                 name:UIKeyboardDidHideNotification object:nil];
	}
	
	#pragma mark ビューが閉じる場合に呼び出される
	-(void) viewWillDisappear:(BOOL)animated {
	     // キーボード表示・非表示時のイベント削除
	    [[NSNotificationCenter defaultCenter] removeObserver:self];
	
	    [super viewWillDisappear:animated];
	}
	
	#pragma mark ビューの設定
	-(void)renderView {
	  [self setTextView];
	}
	
	#pragma mark review UTTextViewの設定
	- (void)setReview {
	    _textView.delegate = self;
	
	    _textView.layer.borderColor = [UIColor lightGrayColor].CGColor;
	    _textView.layer.borderWidth = 1;
	    _textView.clipsToBounds = YES;
	    _textView.layer.cornerRadius = 10.0f;
	
	    // ViewとDoneボタンの作成
	    UIToolbar* keyboardDoneButtonView = [[UIToolbar alloc] init];
	    keyboardDoneButtonView.barStyle  = UIBarStyleBlack;
	    keyboardDoneButtonView.translucent = YES;
	    keyboardDoneButtonView.tintColor = nil;
	    [keyboardDoneButtonView sizeToFit];
	
	    // 完了ボタンとSpacerの配置
	    UIBarButtonItem* doneButton = [[UIBarButtonItem alloc] initWithTitle:@"完了" style:UIBarButtonItemStyleBordered target:self action:@selector(doneBtnClicked)];
	    UIBarButtonItem *spacer1 = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil action:nil];
	    UIBarButtonItem *spacer = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil action:nil];
	    [keyboardDoneButtonView setItems:[NSArray arrayWithObjects:spacer, spacer1, doneButton, nil]];
	
	    // Viewの配置
	    _textView.inputAccessoryView = keyboardDoneButtonView;
	}
	
	#pragma mark 完了ボタンのクリック
	-(void)doneBtnClicked {
	    [_textView resignFirstResponder];
	}
	
	#pragma mark キーボードが表示された時のイベント
	- (void)keyboardWasShown:(NSNotification *)notification {
	    NSDictionary *info  = [notification userInfo];
	    CGSize keyboardSize = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue].size;
	    CGPoint scrollPoint = CGPointMake(0.0f, keyboardSize.height);
	    [_scrollView setContentOffset:scrollPoint animated:YES];
	}
	
	#pragma mark キーボードが閉じた時のイベント
	- (void)keyboardWasHidden:(NSNotification *)notification {
	    [_scrollView setContentOffset:CGPointMake(0.0f, 80.0f) animated:YES];
	}
	
	#pragma mark Doneでキーボードを閉じる
	- (BOOL) textView: (UITextView*) textView shouldChangeTextInRange: (NSRange) range replacementText: (NSString*) text {
	    if ([text isEqualToString:@"\n"]) {
	        [textView resignFirstResponder];
	        return NO;
	    }
	    return YES;
	}
	
	@end


### Return Keyboard Type
	descTextview.returnKeyType = UIReturnKeyDone;


### UITextView cursor change with words

	@property (assign) CGPoint defaultCenter;
	@property (assign) CGFloat reduce;
	@property (assign) CGFloat lastCursorPos;

	- (void)textViewDidBeginEditing:(UITextView *)textView {
	    [self performSelector:@selector(textViewDidChange:) withObject:textView afterDelay:0.1f];
	}
	- (void)textViewDidChange:(UITextView *)textView {
	    CGFloat cursorPosition;
	    if (textView.selectedTextRange) {
	        cursorPosition = [textView caretRectForPosition:textView.selectedTextRange.start].origin.y;
	    } else {
	        cursorPosition = 0;
	    }
	    
	    if (_lastCursorPos != cursorPosition) {
	        _reduce += (cursorPosition - _lastCursorPos);
	        _lastCursorPos = cursorPosition;
	        [UIView animateWithDuration:0.1
	                         animations:^{
	                             self.view.center = CGPointMake(self.view.center.x, self.defaultCenter.y- _reduce);
	                         }];
	    }
	}


	#pragma mark - keyboard hide and show notification
	
	- (void)handleWillShowKeyboard:(NSNotification *)notification {
	    
	    if (KEYBOARD_HEIGHT > descTextview.frame.size.height) {
	        _reduce = KEYBOARD_HEIGHT - descTextview.frame.size.height + 20;
	        [UIView animateWithDuration:0.2
	                         animations:^{
	                             self.view.center = CGPointMake(self.view.center.x, self.defaultCenter.y- _reduce);
	                         }];
	    }
	
	}
	
	- (void)handleWillHideKeyboard:(NSNotification *)notification {
	    [UIView animateWithDuration:0.2 animations:^{
	        self.view.center = self.defaultCenter;
	    }];
	}