### M4a/Mp3 bitrate

1. AVAssetReaderTrackOutput

        NSDictionary *settings = @{
            AVEncoderBitRateKey : @128000,
            AVFormatIDKey : @(kAudioFormatAppleIMA4),
            AVNumberOfChannelsKey : @2,
            AVLinearPCMBitDepthKey : @16,
            AVLinearPCMIsBigEndianKey : @NO,
            AVLinearPCMIsFloatKey : @NO
        };
        AVAssetReaderTrackOutput *assetOutput = [AVAssetReaderTrackOutput assetReaderTrackOutputWithTrack:asset.tracks[0] outputSettings:settings];
        

2. https://github.com/rs/SDAVAssetExportSession
3. https://finalize.com/2014/10/21/exporting-a-video-in-ios-to-reduce-size-and-ensure-maximum-client-compatibility/