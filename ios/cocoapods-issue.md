### cocoaPods issue


It is a known issue in CocoaPods. It has been fixed in version 0.36.1. Just update your CocoaPods and then add specific line of code to your pod file: use_frameworks! after platform :ios, '7.0'

So your file will look like this:

	platform :ios, '7.0'
	
	use_frameworks!
	
	/// here will be dependencies etc. ///


updated:

Full list of steps to get rid of the problem once and for all:

1. Close project;
2. open Terminal App;
3. Update CocoaPods itself to ver. 0.36.1 - you already did this one - you can skip the step this time;
4. navigate to your project folder in Terminal;
5. type: "pod update" (without quotes);
6. wait for updating to finish;
7. Open your project in xCode;
8. Clean project;
9. Build project again.


[ib-designables-failed-to-update-auto-layout-status-failed-to-load-designables](http://stackoverflow.com/questions/28204108/ib-designables-failed-to-update-auto-layout-status-failed-to-load-designables)