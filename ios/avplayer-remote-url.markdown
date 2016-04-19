## AVPlayer with remote mp3

    - (void)playSampleSong:(NSString *)iSongName {
        NSString *aSongURL = [NSString stringWithFormat:@"http://megdadhashem.wapego.ru/files/56727/tubidy_mp3_e2afc5.mp3"];
        // NSLog(@"Song URL : %@", aSongURL);
    
        AVPlayerItem *aPlayerItem = [[AVPlayerItem alloc] initWithURL:[NSURL URLWithString:aSongURL]];
        AVPlayer *anAudioStreamer = [[AVPlayer alloc] initWithPlayerItem:aPlayerItem];
        [anAudioStreamer play];
    
        // Access Current Time
        NSTimeInterval aCurrentTime = CMTimeGetSeconds(anAudioStreamer.currentTime);
    
        // Access Duration
        NSTimeInterval aDuration = CMTimeGetSeconds(anAudioStreamer.currentItem.asset.duration);
    }