## Get button click in table view cell [good]

1. In your `cellForRowAtIndexPath:` method, assign button tag as index:

		cell.yourbutton.tag = indexPath.row;

2. Add target and action for your button as below:

		[cell.yourbutton addTarget:self action:@selector(yourButtonClicked:) forControlEvents:UIControlEventTouchUpInside];
3. Code actions based on index as below in ViewControler:

		-(void)yourButtonClicked:(UIButton*)sender
		{
		     if (sender.tag == 0) 
		     {
		         // Your code here
		     }
		}
Updates for multiple Section:

You can check [this link](http://stackoverflow.com/questions/31649220/detect-button-click-in-table-view-ios-xcode-for-multiple-row-and-section) to detect button click in table view for multiple row and section.

## Links
[Get button click inside UI table view cell](http://stackoverflow.com/questions/20655060/get-button-click-inside-ui-table-view-cell)