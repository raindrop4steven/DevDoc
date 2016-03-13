## Sox
1. Install

		sudo apt-get install lame
		sudo apt-get install sox
		sudo apt-get install libsox-fmt-mp3

1. [Converting_AMR_to_WAV_and_vice_versa](http://www.gentoo-wiki.info/TIP_Converting_AMR_to_WAV_and_vice_versa)

		#!/bin/bash
	
		# By Aquarion - Aquarion@Aquarionics.com
		# Do what you want with it, it's not rocket science.
		
		##Change these:
		#Wherever you put encode and decode when you compiled them:
		CODEC=/usr/local/bin
		
		#Temporary directory. Chances are you've already got this set
		TEMP=/tmp
		
		#Where do the final MP3s go?
		FINAL=/home/user/media/siemens/mp3
		
		for file in *.amr; do 
			FILE=`echo $file | sed -e "s/.amr//"`; 
			echo -n "$FILE [AMR] -> [RAW]"
			$CODEC/decoder $file $TEMP/$FILE.raw > log.std 2> log.err; 
			echo -n " -> [WAV] "
			sox -r 8000 -w -c 1 -s $TEMP/$FILE.raw -r 44100 \
				-w -c 1 $TEMP/$FILE.wav > log.std 2> log.err; 
			echo -n " -> [MP3] "
			lame $TEMP/$FILE.wav $FINAL/$FILE.mp3 --preset standard --silent \
				--tt $FILE --ta Sanda --tl Anul\ Nou --ty `date +%Y`
			echo  " :-) "
			rm $TEMP/$FILE.wav;
			rm $TEMP/$FILE.raw;
		done

2. Convert wav to amr format

		sox input.wav -C 7 ouput.amr-nb 

4. **Play while downloading**
	1.  [AFSoundManager](https://github.com/AlvaroFranco/AFSoundManager)
	2.  [StreamingKit](https://github.com/tumtumtum/StreamingKit)
	3.  [Framer-AudioPlayer](https://github.com/benjaminnathan/Framer-AudioPlayer)
	2.  [AVAssetResourceLoader](https://github.com/leshkoapps/AVAssetResourceLoader)
	3.  [https://github.com/c99koder/lastfm-iphone](https://github.com/c99koder/lastfm-iphone)

5. **Opensource demo **
	1. [MusicPlayerViewController](https://github.com/BeamApp/MusicPlayerViewController)