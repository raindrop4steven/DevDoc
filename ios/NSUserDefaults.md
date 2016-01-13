## NSUserDefaults

###1. Clear All NSUserdefaults

	NSString *appDomain = [[NSBundle mainBundle] bundleIdentifier];
	[[NSUserDefaults standardUserDefaults] removePersistentDomainForName:appDomain];

### 2. Remove one

Try `removeObjectForKey` -- that should give you the ability to remove a preference.

