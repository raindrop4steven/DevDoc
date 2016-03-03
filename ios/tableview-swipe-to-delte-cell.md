### TableView Swipe to delete

	- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
	{
	    // Return YES - we will be able to delete all rows
	    return YES;
	}
	
	- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
	{
	    // Perform the real delete action here. Note: you may need to check editing style
	    //   if you do not perform delete only.
	    NSLog(@"Deleted row.");
		// Delete from coreData
		//
		// Delete from local source
		//
		// Delete from tableView
		[tableView deleteRowAtIndexPaths:[NSArray arrayWithObjects:indexPath] withRowAnimation:UITableViewRowAnimationAutomatic];
	}


### CoreData delete

As @Nektarios said, you are dealing with objects here so you want to find an object that has a particular attribute value. You that with a fetch request and a predicate.

	  NSNumber *soughtPid=[NSNumber numberWithInt:53];
	  NSEntityDescription *productEntity=[NSEntityDescription entityForName:@"Product" inManagedObjectContext:context];
	  NSFetchRequest *fetch=[[NSFetchRequest alloc] init];
	  [fetch setEntity:productEntity];
	  NSPredicate *p=[NSPredicate predicateWithFormat:@"pid == %@", soughtPid];
	  [fetch setPredicate:p];
	  //... add sorts if you want them
	  NSError *fetchError;
	  NSArray *fetchedProducts=[self.moc executeFetchRequest:fetch error:&fetchError];
	  // handle error
The fetchedProducts array will contain all the objects of the entity Product whose pid attribute equals soughtPid. Note that the predicate fulfills the same function logically as the where clause in SQL.

Once you have the objects you just tell the context to delete them:

	  for (NSManagedObject *product in fetchedProducts) {
	    [context deleteObject:product];
	  }
When you next save the context, the object's data will be deleted from the persistent store file.
