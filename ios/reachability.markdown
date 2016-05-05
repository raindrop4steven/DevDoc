## [第三方库Reachability检测网络状态](http://www.hcios.com/archives/1313)

一：确认网络环境3G/WIFI
 
  1. 添加源文件和framework
  
  开发Web等网络应用程序的时候，需要确认网络环境，连接情况等信息。如果没有处理它们，是不会通过Apple的审(我们的)查的。
  Apple 的 例程 Reachability 中介绍了取得/检测网络状态的方法。要在应用程序程序中使用Reachability，首先要完成如下两部：
  
  1.1. 添加源文件：
  在你的程序中使用 Reachability 只须将该例程中的 Reachability.h 和 Reachability.m 拷贝到你的工程中。
Reachability的h和m文件可以在这里下载：https://github.com/tonymillion/Reachability/blob/master/Reachability.h（支持ARC和GCD）
 
  
  1.2.添加framework：
  将SystemConfiguration.framework 添加进工程。如下图：
  
  
  2. 网络状态
  
  Reachability.h中定义了三种网络状态：
  
      typedef enum {
        NotReachable = 0,      //无连接
        ReachableViaWiFi,      //使用3G/GPRS网络
        ReachableViaWWAN      //使用WiFi网络
      } NetworkStatus;
  
  因此可以这样检查网络状态：
 
      Reachability *r = [Reachability reachabilityWithHostName:@“www.apple.com”];
      switch ([r currentReachabilityStatus]) {
          case NotReachable:
              // 没有网络连接
              break;
          case ReachableViaWWAN:
              // 使用3G网络
              break;
          case ReachableViaWiFi:
              // 使用WiFi网络
              break;
      }
  
  3.检查当前网络环境
  程序启动时，如果想检测可用的网络环境，可以像这样
      // 是否wifi
      + (BOOL) IsEnableWIFI {
        return ([[Reachability reachabilityForLocalWiFi] currentReachabilityStatus] != NotReachable);
      }
     
      // 是否3G
      + (BOOL) IsEnable3G {
        return ([[Reachability reachabilityForInternetConnection] currentReachabilityStatus] != NotReachable);
      }
  例子：
  
      - (void)viewWillAppear:(BOOL)animated {  
      if (([Reachability reachabilityForInternetConnection].currentReachabilityStatus == NotReachable) && 
          ([Reachability reachabilityForLocalWiFi].currentReachabilityStatus == NotReachable)) {
          self.navigationItem.hidesBackButton = YES;
          [self.navigationItem setLeftBarButtonItem:nil animated:NO];
        }
      }
 
  4. 链接状态的实时通知
  网络连接状态的实时检查，通知在网络应用中也是十分必要的。接续状态发生变化时，需要及时地通知用户：
  
  Reachability 1.5版本
  
      // My.AppDelegate.h
      #import "Reachability.h"
     
      @interface MyAppDelegate : NSObject <UIApplicationDelegate> {
        NetworkStatus remoteHostStatus;
      }
     
      @property NetworkStatus remoteHostStatus;
     
      @end
     
      // My.AppDelegate.m
      #import "MyAppDelegate.h"
     
      @implementation MyAppDelegate
      @synthesize remoteHostStatus;
     
      // 更新网络状态
      - (void)updateStatus {
        self.remoteHostStatus = [[Reachability sharedReachability] remoteHostStatus];
      }
     
      // 通知网络状态
      - (void)reachabilityChanged:(NSNotification *)note {
        [self updateStatus];
        if (self.remoteHostStatus == NotReachable) {
          UIAlertView *alert = [[UIAlertView alloc] initWithTitle:NSLocalizedString(@"AppName", nil)
                 message:NSLocalizedString (@"NotReachable", nil)
                delegate:nil cancelButtonTitle:@"OK" otherButtonTitles: nil];
          [alert show];
          [alert release];
        }
      }
     
      // 程序启动器，启动网络监视
      - (void)applicationDidFinishLaunching:(UIApplication *)application {
      
        // 设置网络检测的站点
        [[Reachability sharedReachability] setHostName:@"www.apple.com"];
        [[Reachability sharedReachability] setNetworkStatusNotificationsEnabled:YES];
        // 设置网络状态变化时的通知函数
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(reachabilityChanged:)
                             name:@"kNetworkReachabilityChangedNotification" object:nil];
        [self updateStatus];
      }
     
      - (void)dealloc {
        // 删除通知对象
        [[NSNotificationCenter defaultCenter] removeObserver:self];
        [window release];
        [super dealloc];
      } 
  
  Reachability 2.0版本
  
 
      // MyAppDelegate.h
      @class Reachability;
     
        @interface MyAppDelegate : NSObject <UIApplicationDelegate> {
          Reachability *hostReach;
        }
     
      @end
     
      // MyAppDelegate.m
      - (void)reachabilityChanged:(NSNotification *)note {
        Reachability* curReach = [note object];
        NSParameterAssert([curReach isKindOfClass: [Reachability class]]);
        NetworkStatus status = [curReach currentReachabilityStatus];
      
        if (status == NotReachable) {
          UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"AppName""
                   message:@"NotReachable"
                   delegate:nil
                   cancelButtonTitle:@"YES" otherButtonTitles:nil];
                   [alert show];
                   [alert release];
        }
      }
                   
      - (void)applicationDidFinishLaunching:(UIApplication *)application {
        // ...
             
        // 监测网络情况
        [[NSNotificationCenter defaultCenter] addObserver:self
                   selector:@selector(reachabilityChanged:)
                   name: kReachabilityChangedNotification
                   object: nil];
        hostReach = [[Reachability reachabilityWithHostName:@"www.google.com"] retain];
        [hostReach startNotifier];
        // ...
      }
