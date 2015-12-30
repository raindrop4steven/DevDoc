##iOS下的Notification的使用

>在iOS下应用分为两种不同的Notification种类，本地和远程。

###本地Notification

本地的Notification由iOS下NotificationManager统一管理，所使用的对象是UILocalNotification，UILocalNotification的属性涵盖了所有处理Notification需要的内容。

>属性有fireDate、timeZone、repeatInterval、repeatCalendar、alertBody、alertAction、hasAction、alertLaunchImage、applicationIconBadgeNumber、soundName和userInfo。

- 其中fireDate、timeZone、repeatInterval和repeatCalendar是用于UILocalNotification的调度。fireDate是UILocalNotification的激发的确切时间。

- timeZone是UILocalNotification激发时间是否根据时区改变而改变，如果设置为nil的话，那么UILocalNotification将在一段时候后被激发，而不是某一个确切时间被激发。

- repeatInterval是UILocalNotification被重复激发之间的时间差，不过时间差是完全根据日历单位（NSCalendarUnit）的，例如每周激发的单位，NSWeekCalendarUnit，如果不设置的话，将不会重复激发。

- repeatCalendar是UILocalNotification重复激发所使用的日历单位需要参考的日历，如果不设置的话，系统默认的日历将被作为参考日历。

- alertBody、alertAction、hasAction和alertLaunchImage是当应用不在运行时，系统处理。

- alertBody是一串现实提醒内容的字符串（NSString），如果alertBody未设置的话，Notification被激发时将不现实提醒。

- alertAction也是一串字符（NSString），alertAction的内容将作为提醒中动作按钮上的文字，如果未设置的话，提醒信息中的动作按钮将显示为“View”相对文字形式。


- alertLaunchImage是在用户点击提醒框中动作按钮（“View”）时，等待应用加载时显示的图片，这个将替代应用原本设置的加载图片。

- hasAction是一个控制是否在提醒框中显示动作按钮的布尔值，默认值为YES。

- applicationIconBadgeNumber是显示在应用图标右上角的数字，这样让用户直接了解到应用需要处理的Notification。



- soundName是另一个UILocalNotification用来提醒用户的手段，在Notification被激发之后将播放这段声音来提醒用户有Notification需要处理，如果不设soundName的话，Notification被激发是将不会有声音播放，除去应用特制的声音以外，也可以将soundName设为UILocalNotificationDefaultSoundName来使用系统默认提醒声音。

- userInfo是Notification用来传递数据的NSDictionary。

 

### 封装

1. 在项目需要开启定时提醒的地方设置UILocalNotification对象,在系统Notification处理队列中登记已设置完的UILocalNotification对象：  

		NSInteger period = 7200;//设置唤醒期间 60*60*2 两个小时后提醒
		UILocalNotification *notification=[[UILocalNotification alloc] init];
        if (notification!=nil)  {
            
            NSDate *now=[NSDate new];
           
            //notification.fireDate=[now addTimeInterval:period];
            notification.fireDate = [now dateByAddingTimeInterval:period];
            NSLog(@"%d",period);
            notification.timeZone=[NSTimeZone defaultTimeZone];
            notification.soundName = @"ping.caf";
            //notification.alertBody=@"TIME！";
            
            notification.alertBody = [NSString stringWithFormat:@"@%时间到了!",nameStr];
            
            NSDictionary* info = [NSDictionary dictionaryWithObject:uniqueCodeStr forKey:CODE];
            notification.userInfo = info;


            /**在设置完UILocalNotification对象之后，应用需要在系统Notification处理队列中登记已设置完的UILocalNotification对象。*/

            [[UIApplication sharedApplication] scheduleLocalNotification:notification];      
        } 

        [notification release];

2. 如果你的应用程序处于关闭状态，设置的时间到了，会自动在桌面弹出一个提示框，点显示后，就可以启动软件。然后在这里对lanchOptions进行处理，找到它里面的信息,就可以拿到设置时的需要处理的东西，就可以继续操作了。

    	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
			UILocalNotification *localNotif =
        	[launchOptions objectForKey:UIApplicationLaunchOptionsLocalNotificationKey];
			if (localNotif) {
				NSLog(@"Recieved Notification %@",localNotif);
				NSDictionary* infoDic = localNotif.userInfo;
				NSLog(@"userInfo description=%@",[infoDic description]);
				NSString* codeStr = [infoDic objectForKey:CODE];
				}
			}


3. 如果此时你的客户端 软件处于打开状态，则会调用  

		- (void)application:(UIApplication *)app didReceiveLocalNotification:(UILocalNotification *)notif {
		//同上一样的处理方法一样的处理方法。
		if (notif) {
			NSLog(@"Recieved Notification %@",notif);
			NSDictionary* infoDic = notif.userInfo;
			NSLog(@"userInfo description=%@",[infoDic description]);
			NSString* codeStr = [infoDic objectForKey:CODE];
			}
		}

### 两个方式取消一个已经登记的Notification

1. 第一个方式可以直接取消一个指定的Notification:

		NSString *myIDToCancel = @"some_id_to_cancel"; 
		UILocalNotification *notificationToCancel=nil; 
		for(UILocalNotification *aNotif in [[UIApplication sharedApplication] scheduledLocalNotifications]) {   
			if([aNotif.userInfo objectForKey:@"ID"] isEqualToString:myIDToCancel]) {      
				notificationToCancel=aNotif;    
				break;   
			} 
		} 
		[[UIApplication sharedApplication] cancelLocalNotification:notificationToCancel];

2. 第二个方式将会把该应用已登记的Notification一起取消:

		[[UIApplication sharedApplication] cancelAllLocalNotification];

### 将UILocalNotification封装在单独的类中进行使用
####封装实现

	// LocalNotificationsManager.h 文件内容
	
	 #import <Foundation/Foundation.h>
	
	 @interface LocalNotificationsManager : NSObject
	
	 + (void)addLocalNotificationWithFireDate:(NSDate *)date 
	
	                              activityId:(NSInteger)aid 
	
	                           activityTitle:(NSString *)title;
	
	 
	 + (BOOL)removeLocalNotificationWithActivityId:(NSInteger)aid;
	
	@end



	//  LocalNotificationsManager.m 文件内容

	 #import "LocalNotificationsManager.h"
	
	 @implementation LocalNotificationsManager
	
	 + (void)addLocalNotificationWithFireDate:(NSDate *)date 　　　　　　//设置提醒
	
	                              activityId:(NSInteger)aid 
	
	                           activityTitle:(NSString *)title {
	
	    NSDate *t = [NSDatedateWithTimeIntervalSinceNow:7200.0f];      // 2*60*60 2h
	
	    NSComparisonResult result = [t compare:date];
	
	    if (result == -1) {
	
	        UILocalNotification *notification=[[UILocalNotificationalloc] init]; 
	
	        notification.fireDate = [date dateByAddingTimeInterval:-7200.0f]; 
	
	        notification.timeZone = [NSTimeZone defaultTimeZone]; 
	
	        notification.applicationIconBadgeNumber = 1;
	
	        notification.soundName = UILocalNotificationDefaultSoundName;
	
	        notification.alertBody = [NSString stringWithFormat:@"%@ 活动马上就要开始了",title]; 
	
	        NSDictionary *dic =
	
	         [NSDictionarydictionaryWithObjectsAndKeys:[NSNumbernumberWithInt:aid], @"activityid", nil];
	
	        notification.userInfo = dic;
	
	        [[UIApplicationsharedApplication] scheduleLocalNotification:notification];
	
	        [notification release];
	
	    }
	
	}
	
	 + (BOOL)removeLocalNotificationWithActivityId:(NSInteger)aid //取消提醒
	
	{
	
	    UIApplication *application = [UIApplicationsharedApplication];
	
	    NSArray *localNotifications = [[UIApplicationsharedApplication] scheduledLocalNotifications];
	
	    for (UILocalNotification *obj in localNotifications) {
	
	        NSInteger activityId = [[obj.userInfo objectForKey:@"activityid"] intValue];
	
	        if (aid == activityId) {
	
	            [application cancelLocalNotification:obj];
	
	            return YES;
	
	        }
	
	    }
	
	    returnNO;
	
	}
	
	@end

 

#### 使用方法

1. 导入  "LocalNotificationsManager.h"类

2. 定时提醒

		[LocalNotificationsManager addLocalNotificationWithFireDate:[NSDategetDateTimeFromString:self.creatorActivity.beginDate] 　　　//beginDate（*NSString）
                                                         activityId:aid 
                                                      activityTitle:self.creatorActivity.activityName];

3. 取消提醒

		[LocalNotificationsManagerremoveLocalNotificationWithActivityId:self.activity.activityID];