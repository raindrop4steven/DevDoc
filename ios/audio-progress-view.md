这有一个例子：
iPhone – Add an UISlider For AVAudioPlayer
We can use AVAudioPlayer to play music. Now i want to add an UISlider as the music progress bar and user can fast skip the music.

Now i have a play button in the view and when user click it, i will play the music file as well as the following method.

    * playButtonClicked – Run when the play button is clicked
    * updateSlider – Run in 1 second interval to update the UISlider
    * sliderChange – Fast skip the music when user scroll the UISlider

Here comes the code - (IBAction)playButtonClicked:(id)sender {

        // Read the file from resource folder and set it in the avAudioPlayer

        NSURL *fileUrl = [[NSURL alloc] initFileURLWithPath:[[NSBundle mainBundle] pathForResource:@"musicFile" ofType:@"caf"]];

        avAudioPlayer = [[AVAudioPlayer alloc] initWithContentsOfURL:fileUrl error:nil];

        [avAudioPlayer setDelegate:self];

        [avAudioPlayer setVolume:1.0];



        // Set a timer which keep getting the current music time and update the UISlider in 1 sec interval

        sliderTimer = [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(updateSlider) userInfo:nil repeats:YES];

        // Set the maximum value of the UISlider

        aSlider.maximumValue = avAudioplayer.duration;

        // Set the valueChanged target

        [aSlider addTarget:self action:@selector(sliderChanged:) forControlEvents:UIControlEventValueChanged];



        // Play the audio

        [avAudioPlayer prepareToPlay];

        [avAudioPlayer play];

}



- (void)updateSlider {

        // Update the slider about the music time

        aSlider.value = avAudioPlayer.currentTime;

}



- (IBAction)sliderChanged:(UISlider *)sender {

        // Fast skip the music when user scroll the UISlider

        [avAudioPlayer stop];

        [avAudioPlayer setCurrentTime:aSlider.value];

        [avAudioPlayer prepareToPlay];

        [avAudioPlayer play];

}



// Stop the timer when the music is finished (Need to implement the AVAudioPlayerDelegate in the Controller header)

- (void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag {

        // Music completed

        if (flag) {

                [sliderTimer invalidate];

        }

}
复制代码
