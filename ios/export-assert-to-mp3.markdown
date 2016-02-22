### Export Session to MP3
```objc
     MPMediaItem *mediaItem = [_mediaItems objectAtIndex:_count];
    
     //get the name of the file.
     NSString *songTitle = [mediaItem valueForProperty: MPMediaItemPropertyTitle];
    
     //convert MPMediaItem to AVURLAsset.
     AVURLAsset *sset = [AVURLAsset assetWithURL:[mediaItem valueForProperty:MPMediaItemPropertyAssetURL]];
    
    //get the extension of the file.
    NSString *fileType = [[[[sset.URL absoluteString] componentsSeparatedByString:@"?"] objectAtIndex:0] pathExtension];
    
    //init export, here you must set "presentName" argument to "AVAssetExportPresetPassthrough". If not, you will can't export mp3 correct.
    AVAssetExportSession *export = [[AVAssetExportSession alloc] initWithAsset:sset presetName:AVAssetExportPresetPassthrough];
    
    NSLog(@"export.supportedFileTypes : %@",export.supportedFileTypes);
    //export to mov format.
    export.outputFileType = @"com.apple.quicktime-movie";
    
    export.shouldOptimizeForNetworkUse = YES;
    
    NSString *extension = ( NSString *)UTTypeCopyPreferredTagWithClass(( CFStringRef)export.outputFileType, kUTTagClassFilenameExtension);
    
    NSLog(@"extension %@",extension);
    NSString *path = [NSHomeDirectory() stringByAppendingFormat:@"/Documents/%@.%@",songTitle,extension];
    
    NSURL *outputURL = [NSURL fileURLWithPath:path];
    export.outputURL = outputURL;
    [export exportAsynchronouslyWithCompletionHandler:^{
    
       if (export.status == AVAssetExportSessionStatusCompleted)
         {
            //then rename mov format to the original format.
             NSFileManager *manage = [NSFileManager defaultManager];
    
             NSString *mp3Path = [NSHomeDirectory() stringByAppendingFormat:@"/Documents/%@.%@",songTitle,fileType];
    
             NSError *error = nil;
    
             [manage moveItemAtPath:path toPath:mp3Path error:&error];
    
             NSLog(@"error %@",error);
    
          }
          else
          {
             NSLog(@"%@",export.error);
          }
    
    }];
```

[How to Export Mp3 From Ipod-Library](http://tuchangwei.github.io/2013/06/04/how-to-export-mp3-from-ipod-library/)