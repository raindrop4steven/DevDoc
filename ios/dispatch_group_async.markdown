## Dispatch_group_async

    dispatch_group_t group = dispatch_group_create();
    
    dispatch_group_async(group, dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), 
    ^{
         //Code here is executed asynchronously
    });
    
    dispatch_group_notify(group, dispatch_get_main_queue(), 
    ^{
       //Do something when async has completed
       //Note: You are not required to use the main 
       //queue if you aren't performing any UI work.
    });
    
    dispatch_release(group);