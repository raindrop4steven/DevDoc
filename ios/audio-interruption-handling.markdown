## iOS interruption handling

Implement the audioPlayerBeginInterruption: and audioPlayerEndInterruption:withFlags: methods of the AVAudioPlayerDelegate protocol in the delegate object of your AVAudioPlayer instance:

    - (void)audioPlayerBeginInterruption:(AVAudioPlayer *)player{
      
      /* Audio Session is interrupted. The player will be paused here */
      
    }- (void)audioPlayerEndInterruption:(AVAudioPlayer *)player 
                             withFlags:(NSUInteger)flags{
      
      if (flags == AVAudioSessionInterruptionFlags_ShouldResume &&
          player != nil){
        [player play];
      }
      
    }