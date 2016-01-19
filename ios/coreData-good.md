### 1. CoreData 好文

1. [CoreData 好文](http://www.cnblogs.com/xiaodao/archive/2012/10/08/2715477.html)
2. [Predicate for Query CoreData](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Predicates/Articles/pUsing.html)
### 2. Get AppDelegate
You can access the delegate like this:

	MainClass *appDelegate = (MainClass *)[[UIApplication sharedApplication] delegate];
Replace MainClass with the name of your application class.

Then, provided you have a property for the other view controller, you can call something like:

	[appDelegate.viewController someMethod];

### 3. NSString to NSNumber

	NSNumberFormatter *f = [[NSNumberFormatter alloc] init];
	f.numberStyle = NSNumberFormatterDecimalStyle;
	NSNumber *myNumber = [f numberFromString:@"42"];

### 4.

 **[CoreData version different](http://stackoverflow.com/questions/8881453/the-model-used-to-open-the-store-is-incompatible-with-the-one-used-to-create-the)**

### 5. Predicate Query CoreData

**CAUTIOn**: NSNumber使用%@,而不是%ld

	NSFetchRequest *fetch = [NSFetchRequest fetchRequestWithEntityName:@"Person"];
	
	NSString *nameToGet = @"Ryan";
	NSPredicate *predicate = [NSPredicate predicateWithFormat:@"name = %@", nameToGet];
	[fetch setPredicate:predicate];
	
	NSError *error = nil;
	NSArray *results = [[self managedObjectContext] executeFetchRequest:fetch error:&error];
	if(results) {
	    NSLog(@"Entities with that name: %@", results);
	    for(Person *p in results) {
	        NSLog(@"person = %@", p);
	    }
	    return results;
	} else {
	    NSLog(@"Error: %@", error);
	}
	
	return nil;

### Query All data from CoreData

	NSManagedObjectContext *context = //Get it from AppDelegate
	
	NSFetchRequest *request = [[NSFetchRequest alloc]initWithEntityName:@"MUSTHAFA"];
	
	NSError *error = nil;
	
	NSArray *results = [context executeFetchRequest:request error:&error];
	
	if (error != nil) {
	
	   //Deal with failure
	}
	else {
	
	   //Deal with success
	}