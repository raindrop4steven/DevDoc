## Record permission

Please note that this will only work if built with Xcode 5, and not with 4.6

Add the AVFoundation Framework to your project

Then import the AVAudioSession header file, from the AVFoundation framework, where you intend to check if the microphone setting is enabled

    #import <AVFoundation/AVAudioSession.h>
Then simply call this method

    [[AVAudioSession sharedInstance] requestRecordPermission:^(BOOL granted) {
                if (granted) {
                    // Microphone enabled code
                }
                else {
                    // Microphone disabled code
                }
            }];
The **first time** this method runs, it will **show the prompt to allow microphone access and based on the users response it will execute the completion block. From the second time onwards it will just act based on the stored setting on the device.**


## Take me to settings

    // Send the user to the Settings for this app
    NSURL *settingURL = [NSURL URLWithString:UIApplicationOpenSettingsURLString];
    [[UIApplication sharedApplication] openURL:settingURL];