### Using `self` in block

	// Make it weak first
	__unsafe_unretained typeof(self) weakSelf = self;
	[player addPeriodicTimeObserverForInterval:CMTimeMakeWithSeconds(0.1, 100)
	                                     queue:nil
	                                usingBlock:^(CMTime time) {
	                                                current+=1;
	
	                                                if(current==60)
	                                                {
	                                                    min+=(current/60);
	                                                    current = 0;
	                                                }
	
	                                                 [weakSelf.timerDisp setText:[NSString stringWithFormat:@"%02d:%02d",min,current]];
	                                            }];