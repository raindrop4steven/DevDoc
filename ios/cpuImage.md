### 1. Login&Register

1. GPUImage 高斯模糊

		- (UIImage *)blurryGPUImage:(UIImage *)image 
		              withBlurLevel:(NSInteger)blur {
		    GPUImageFastBlurFilter *blurFilter = 
		                        [[GPUImageFastBlurFilter alloc] init];
		    blurFilter.blurSize = blur;
		    UIImage *result = [blurFilter imageByFilteringImage:image];
		    return result;
		}


### 2. Blur under view

	//only apply the blur if the user hasn't disabled transparency effects
	if (!UIAccessibilityIsReduceTransparencyEnabled()) {
	    self.view.backgroundColor = [UIColor clearColor];
	
	    UIBlurEffect *blurEffect = [UIBlurEffect effectWithStyle:UIBlurEffectStyleDark];
	    UIVisualEffectView *blurEffectView = [[UIVisualEffectView alloc] initWithEffect:blurEffect];

		//always fill the view
	    blurEffectView.frame = self.view.bounds;
	    blurEffectView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
	
		//if you have more UIViews, use an insertSubview API to place it where needed
	    [self.view addSubview:blurEffectView];
	}  
	else {
	    self.view.backgroundColor = [UIColor blackColor];
	}

### 3. 连接

1. [http://iosicongallery.com/](http://iosicongallery.com/)