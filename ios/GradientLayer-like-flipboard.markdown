### CAGradientLayer:

Add `QuartzCore.framework` to your project. 

Import the QuartzCore header:

    #import <QuartzCore/QuartzCore.h>
    
Add a CAGradientLayer to the view in question. For example, for a simple gradient from gray to white:

    - (void)addGradientToView:(UIView *)view
    {
        CAGradientLayer *gradient = [CAGradientLayer layer];
        gradient.frame = view.bounds;
        gradient.colors = @[(id)[[UIColor lightGrayColor] CGColor],
                            (id)[[UIColor whiteColor] CGColor]];
        [view.layer insertSublayer:gradient atIndex:0];
    }

Or from one shade of gray, to another, and back:

    - (void)addGradientToView:(UIView *)view
    {
        CAGradientLayer *gradient = [CAGradientLayer layer];
        gradient.frame = view.bounds;
        gradient.colors = @[(id)[[UIColor colorWithRed:0.8 green:0.8 blue:0.8 alpha:1.0] CGColor],
                            (id)[[UIColor colorWithRed:0.9 green:0.9 blue:0.9 alpha:1.0] CGColor],
                            (id)[[UIColor colorWithRed:0.8 green:0.8 blue:0.8 alpha:1.0] CGColor]];
        [view.layer insertSublayer:gradient atIndex:0];
    }