## tip view

1. [create-click-through-overlay-instruction-view](http://stackoverflow.com/questions/12289756/create-click-through-overlay-instruction-view)


        - (BOOL)application:(UIApplication *)application 
        didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {  
        
            UIImageView *imageView = [[UIImageView alloc] 
            initWithImage:[UIImage imageNamed:@"yourimage.png"]];
            [self.window addSubview:imageView];
        
             UITapGestureRecognizer * recognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(handleTap:)];
             recognizer.delegate = self;
            [imageView addGestureRecognizer:recognizer];
            imageView.userInteractionEnabled =  YES;
            self.imageView = imageView;
        }
        
        - (void) handleTap:(UITapGestureRecognizer *)recognize
        {
             [self.imageView removeFromSuperView];
        }
        

