### 1. Resources to Documents
1. plist to store preinstalled music
	1. | id | Name | FileName | Category |
	2. In `applicationDidLauchSucceed`, create Documents/UserName/Music Dir first then copy local resource into dest dir, save info into CoreData. 
	3. first time run
		    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];

		    if ( ![userDefaults valueForKey:@"version"] )
		    {
		            // CALL your Function;
		
		            // Adding version number to NSUserDefaults for first version:
		          [userDefaults setFloat:[[[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleVersion"] floatValue] forKey:@"version"];    
		    }
		
		
		    if ([[NSUserDefaults standardUserDefaults] floatForKey:@"version"] == [[[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleVersion"] floatValue] )
		    {
		    /// Same Version so dont run the function
		    }
		    else
		    {
		       // Call Your Function;
		
		       // Update version number to NSUserDefaults for other versions:
		         [userDefaults setFloat:[[[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleVersion"] floatValue] forKey:@"version"];
		    }
### 2. Local music and online music
1. Local music
	- Shown by category (group), easy choice.
	- Frequent music(3), local music list

2. Online music
	- Shown by popularity(order by user's favor + popularity)
	- download once and update popularity in database.
	- download and save into Documents, then insert this record in coredata, reload localmusic.

3. CoreData
	1. Local Music
		1. id NSInteger == mid (check if already downlaoded)
		2. **fileName** NSString - Generated in saving process
		3. name NSString == music_name (shown for user)
		4. category == category (shown for user)
		
	2. Music Table:
		1. First five records in table matches them in plist, so that after copy you could manage them as downloaded resource.
		2. The online music also shows them.

	3. Local Music Name
		1. Category-mid.mp3
		