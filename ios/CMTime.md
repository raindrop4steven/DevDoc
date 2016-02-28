## CMTime to seconds

	NSUInteger dTotalSeconds = CMTimeGetSeconds(durationV);

## seconds to CMTime
	CMTimeMakeWithSeconds(CMTimeGetSeconds(music.currentTime) + 5, music.currentTime.timescale);
