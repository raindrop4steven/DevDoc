### [AVAssetExportSession - Exporting a Trimmed Audio Asset](https://developer.apple.com/library/ios/qa/qa1730/_index.html)

    - (BOOL)exportAsset:(AVAsset *)avAsset toFilePath:(NSString *)filePath {
    
        // we need the audio asset to be at least 50 seconds long for this snippet
        CMTime assetTime = [avAsset duration];
        Float64 duration = CMTimeGetSeconds(assetTime);
        if (duration < 50.0) return NO;
    
        // get the first audio track
        NSArray *tracks = [avAsset tracksWithMediaType:AVMediaTypeAudio];
        if ([tracks count] == 0) return NO;
    
        AVAssetTrack *track = [tracks objectAtIndex:0];
    
        // create the export session
        // no need for a retain here, the session will be retained by the
        // completion handler since it is referenced there
        AVAssetExportSession *exportSession = [AVAssetExportSession
                                               exportSessionWithAsset:avAsset
                                               presetName:AVAssetExportPresetAppleM4A];
        if (nil == exportSession) return NO;
    
        // create trim time range - 20 seconds starting from 30 seconds into the asset
        CMTime startTime = CMTimeMake(30, 1);
        CMTime stopTime = CMTimeMake(50, 1);
        CMTimeRange exportTimeRange = CMTimeRangeFromTimeToTime(startTime, stopTime);
    
        // create fade in time range - 10 seconds starting at the beginning of trimmed asset
        CMTime startFadeInTime = startTime;
        CMTime endFadeInTime = CMTimeMake(40, 1);
        CMTimeRange fadeInTimeRange = CMTimeRangeFromTimeToTime(startFadeInTime,
                                                                endFadeInTime);
    
        // setup audio mix
        AVMutableAudioMix *exportAudioMix = [AVMutableAudioMix audioMix];
        AVMutableAudioMixInputParameters *exportAudioMixInputParameters =
                [AVMutableAudioMixInputParameters audioMixInputParametersWithTrack:track];
    
        [exportAudioMixInputParameters setVolumeRampFromStartVolume:0.0 toEndVolume:1.0
                                       timeRange:fadeInTimeRange]; 
        exportAudioMix.inputParameters = [NSArray
                                          arrayWithObject:exportAudioMixInputParameters]; 
    
        // configure export session  output with all our parameters
        exportSession.outputURL = [NSURL fileURLWithPath:filePath]; // output path
        exportSession.outputFileType = AVFileTypeAppleM4A; // output file type
        exportSession.timeRange = exportTimeRange; // trim time range
        exportSession.audioMix = exportAudioMix; // fade in audio mix
    
        // perform the export
        [exportSession exportAsynchronouslyWithCompletionHandler:^{
    
            if (AVAssetExportSessionStatusCompleted == exportSession.status) {
                NSLog(@"AVAssetExportSessionStatusCompleted");
            } else if (AVAssetExportSessionStatusFailed == exportSession.status) {
                // a failure may happen because of an event out of your control
                // for example, an interruption like a phone call comming in
                // make sure and handle this case appropriately
                NSLog(@"AVAssetExportSessionStatusFailed");
            } else {
                NSLog(@"Export Session Status: %d", exportSession.status);
            }
        }];
    
        return YES;
    }