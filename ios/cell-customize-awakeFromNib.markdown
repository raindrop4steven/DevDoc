### Cell Customize

    - (void)awakeFromNib {
        // Initialization code
        // darken album view
        CAGradientLayer *gradient = [CAGradientLayer layer];
        gradient.frame = self.bounds;
        gradient.colors = gradient.colors = [NSArray arrayWithObjects:(id)[[UIColor clearColor] CGColor], (id)[[UIColor colorWithWhite:0 alpha:0.9] CGColor], nil];
        [self.albumView.layer insertSublayer:gradient atIndex:0];
        
        // content Label font color
        self.contentLabel.textColor = [UIColor whiteColor];
    }