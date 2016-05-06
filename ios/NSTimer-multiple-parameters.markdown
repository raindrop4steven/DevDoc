## NSTimer multiple parameters

    NSMutableDictionary *myDictionary = [[NSMutableDictionaryalloc] initWithObjectsAndKeys:@"value1",@"table",@"value2",@"indexPath",nil];
    
    [NSTimer scheduledTimerWithTimeInterval:0.5
                                     target:self
                                   selector:@selector(onTimer:)
                                   userInfo:myDictionary
                                    repeats:NO];
    
    - (void)onTimer:(NSTimer *)timer
    {
        NSLog(@"--- %@", [timer userInfo] );
    }