## 用户体验

1. [用户为王 - 关于征询授权、注册及加载等待的体验优化](http://www.cocoachina.com/design/20150610/12096.html)
2. [在正确的情境中向用户获取iOS权限](http://www.beforweb.com/node/468)


### Raindrop flow
1. 先用空View代替NearView，等到滑动到该View时再替换成真正的View，同时可以弹出授权。
2. Profile界面中个人信息部分使用一个Register/Login View来替换，然后可以弹出登录。
3. Add-如果未登录则首先弹出登录界面，然后


### Problem to solve
1. Detail scroll, see misc on github
2. [iRate](https://github.com/nicklockwood/iRate)
3. UI

4. AVAudioPlayer

		NSInteger currentSoundsIndex; //Don't forget to set this in viewDidLoad or elsewhere
		
		//In viewDidLoad add this line
		{
		...
		currentSoundsIndex = 0;
		...
		}
		
		-(void) playCurrentSong
		{
		NSError *error;
		mediaPlayer = [[AVAudioPlayer alloc] initWithContentsOfURL:[[NSURL alloc] initFileURLWithPath:[[NSBundle mainBundle] pathForResource:[soundList objectAtIndex:currentSoundsIndex] ofType:nil]] error:&error];
		if(error !=nil)
		{
		   NSLog(@"%@",error);
		   //Also possibly increment sound index and move on to next song
		}
		else
		{
		self.lblCurrentSongName.text = [soundList objectAtIndex:currentSoundsIndex];
		[mediaPlayer setDelegate:self];
		[mediaPlayer prepareToPlay]; //This is not always needed, but good to include
		[mediaPlayer play];
		}
		
		}
		
		//This is the delegate method called after the sound finished playing, there are also other methods be sure to implement them for avoiding possible errors
		- (void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag
		{
		//Increment index but don't go out of bounds
		currentSoundsIndex = ++currentSoundsIndex % [soundList count];
		[self playCurrentSong];
		}

5. [【初心者でも簡単】facebookのPopでiOSアニメーションやってみた](http://tech.voyagegroup.com/archives/7679085.html)
6. [Facebook POP 进阶指南](http://www.cocoachina.com/industry/20140704/9034.html)