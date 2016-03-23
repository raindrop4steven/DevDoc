## Override default navigation back action

    -(void) viewWillDisappear:(BOOL)animated {
        if ([self.navigationController.viewControllers indexOfObject:self]==NSNotFound) {
           // Navigation button was pressed. Do some stuff 
          [self.navigationController popViewControllerAnimated:NO];
        }
        [super viewWillDisappear:animated];
    }
    
## Change global barButtonColor

    
    // Set NavigationBar white background
    [self.navigationController.navigationBar setBarTintColor:[UIColor whiteColor]];
    [self.navigationController.navigationBar setTranslucent:NO];
    
    // Set BarButtonItem color
    [[self.navigationController navigationBar] setTintColor:[UIColor raindropBlueColor]];