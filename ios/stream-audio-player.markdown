## Streaming Audio

1. [AudioStreamer](https://github.com/mattgallagher/AudioStreamer)
2. [FreeStreamer](https://github.com/muhku/FreeStreamer)


## [Fix StreamingKit](https://github.com/tumtumtum/StreamingKit/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+dataSourceEof+)

By adding the following code in the **AppDelegate.m**, it works now.
    
    NSError* error;
    
    [[AVAudioSession sharedInstance] setCategory:AVAudioSessionCategoryPlayback error:&error];
    [[AVAudioSession sharedInstance] setActive:YES error:&error];
    