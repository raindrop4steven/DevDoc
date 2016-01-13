# ios 防止按钮快速点击造成多次响应的避免方法。


有时候有些操作是防止用户在一次响应结束中再响应下一个。但有些测试用户就要猛点，狂点。像这种恶意就要进行防止。
当然有些异步操作时，可以在调用前enable 掉。等CallBACK 后再enable起来。过程中按钮是不能点的。

1、可以使用：
	- (void) timeEnough
	{
	 
	UIButton *btn=(UIButton*)[self.view viewWithTag:33];
	 
	btn.selected=NO; 
	[timer invalidate];
	 
	timer=nil; 
	}

	 - (void) btnDone:(UIButton*)btn
	 {
	 if(btn.selected) return;
	 btn.selected=YES;
	 [self performSelector:@selector(timeEnough) withObject:nil afterDelay:3.0]; //使用延时进行限制。
	//to do something.
	 }

2、个人觉得这种方法更为好用些。

	- (void)todoSomething:(id)sender
	{
	    //在这里做按钮的想做的事情。
	}
	
	- (void)starButtonClicked:(id)sender
	{
	    //先将未到时间执行前的任务取消。
	    [[self class] cancelPreviousPerformRequestsWithTarget:self selector:@selector(todoSomething:) object:sender];
	    [self performSelector:@selector(todoSomething:) withObject:sender afterDelay:0.2f];
	}

对于第二种方法，快速点击Ｎ次，只要每次间隔在0.2秒内的都不响应操作，等到停下点击到达0.2秒后再执行。所以按照自己的需要设置响应时间，狂点吧。只响应一次。。