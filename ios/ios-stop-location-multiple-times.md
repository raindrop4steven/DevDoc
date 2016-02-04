### iOS LocationManager 多次定位

#### 起因
使用iOS 自带的LocationManager在获取位置后会出现多次定位，使得UIViewController多次更新。

#### 解决：

##### 1. 将LocationManager的初始化放在`- (void)viewWillAppear:Animated`中，然后在获得地理位置后将Manager.delegate = nil.
	
	- (void)viewWillAppear:animated {
		[super viewWillAppear:animated];

		if (_locationManager == nil) {
			_locationManager = [[CLLocationManager alloc] init];
			_locationManager.delegate = self;
		}
	}

	- (void)locationManager:(CLLocationManager *) manager didGetLocationSuccess:(CLocation *)location {
		if(validate(location)){
			[_locationManager stopUpdateLocation];
			_locationManager = nil;
			_locationManager.delegate = nil;

			_longitute = location.longitude;
			_latitude = location.latitude;
			[self reloadVoiceData];
		}
	}