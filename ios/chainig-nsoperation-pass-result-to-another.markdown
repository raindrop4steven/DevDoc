## Chaining **NSOperation** : Pass result from an operation to the next one

### Requirements

1. Operation1 to download JSON data from server
2. Operation2 to parse & model JSON received
3. Operation3 to download user images


### Solution

    DownloadOperation *downloadOperation = [[DownloadOperation alloc] initWithURL:url];
    ParseOperation *parseOperation = [[ParseOperation alloc] init];
    DownloadImagesOperation *downloadImagesOperation = [[DownloadImagesOperation alloc] init];
    
    downloadOperation.downloadCompletionHandler = ^(NSData *data, NSError *error) {
        if (error != nil) {
            NSLog(@"%@", error);
            return;
        }
    
        parseOperation.data = data;
        [queue addOperation:parseOperation];
    };
    
    parseOperation.parseCompletionHandler = ^(NSDictionary *dictionary, NSError *error) {
        if (error != nil) {
            NSLog(@"%@", error);
            return;
        }
    
        NSArray *images = ...;
    
        downloadImagesOperation.images = images;
        [queue addOperation:downloadImagesOperation];
    };
    
    [queue addOperation:downloadOperation];
    

### qiniu的坑

[qiniu的坑](http://www.pchou.info/index.html)