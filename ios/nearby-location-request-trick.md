### Lazy login/register

#### 1. When to show login/register

1. Mine: duiTang Profile
2. After selecting motions


#### 2. [Check location authorization status](http://stackoverflow.com/questions/7221004/detecting-whether-location-services-are-enabled-for-my-app)

	[CLLocationManager authorizationStatus] != kCLAuthorizationStatusDenied

#### 3. 逼死处女座的地理位置请求,类似片刻的动态页面里的设置

1. When first logged in, show an instruction page in **NearBy** as hint: Permit for location for nearby voices.
2. Implementation NearViewController:
		
		// 逼死处女座
		- (void)ViewWillAppear:animated
		{
			// Permission allowed, so just fetch nearby data
			if ([CLLocationManager authorizationStatus] != kCLAuthorizationStatusDenied)
			{
				[self askForLocation];
			}
			else
			{
				// Show a blank page with permission hints:
				// To see friends around you, please turn on the location on.
				// Button on it :)
				HintView *hintView = [HintView new];
				hintView.tag = 1;
				[self.view addSubview: hintView];
			}
		}

		// CLLocationManagerDelegate
		- (void)CLLocationManagerDidGetLocationSuccess:(CCLocation *)location
		{
			// Get the hintView and remove it from self.view
			HintView *hintView = [self.view viewWithTag:1];
			[hintView removeFromParentView];

			// Get longitude and latitude and then reload data.
			// :)
		}
		





#### 4. [core-location-manager-changes-in-ios-8](http://nevan.net/2014/09/core-location-manager-changes-in-ios-8/)