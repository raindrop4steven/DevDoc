
## Linux ffmpeg and libmp3lame

1. Add repository for ffmpeg

        sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
        sudo apt-get update
        sudo apt-get install ffmpeg
        
2. Install libmp3lame

        sudo apt-get install ffmpeg libavcodec-extra-54
        
3. Conversion from m4a to mp3

        ffmpeg -i 20160310204434341.m4a -acodec libmp3lame -ab 128k result.mp3
        
## [iOS下使用FFMPEG的一些总结](http://blog.sina.com.cn/s/blog_47522f7f0102vbwp.html)

1. [ffmpeg编译脚本](https://github.com/kewlbear/FFmpeg-iOS-build-script)
    > `./build-ffmpeg.sh`
    脚本则会自动从github中把ffmpeg源码下到本地并开始编译。
    其中，ffmpeg-2.5.3是源码，FFmpeg-iOS是编译出来的库，里面有我们需要的.a静态库，一共有7个。
    执行命令：
    `lipo -info libavcodec.a`
    查看.a包支持的架构，这几个包都支持了armv7 armv7s i386 x86_64 arm64这几个架构，这个脚本果真是业界良心啊～～～
    
2. xcode中引入FFMPEG library库
>新建工程，把上面编译好的FFmpeg-iOS拖到xcode工程中，添加一个头文件引用`#include "avformat.h"`
添加一个api语句：`av_register_all();`
添加一个空的类，把执行文件.m后缀改为.mm，开启混编模式。
添加相应的framework，包括avfoundation和coremedia。
运行工程，如果没有报错，则表明编译成功。

3. 在xcode项目中使用命令行
执行到第4步，已经可以使用library库了。但是如果要对视频进行操作，还是需要手动写很多代码去调用api，工作量较大，自然不如直接写命令行方便。为了命令行能够在xcode工程中使用，还需要做以下工作：
    - 添加源码中的tools,具体文件包括：

    - 添加Header Search Paths
        在target--build setting中搜索Header Search Paths，并在Header Search Paths下面添加源码ffmpeg-2.5.3和scratch的路径。
    - 修改ffmpeg.h和ffmpeg.c源码
>如果此时run这个工程，则会报错，原因是工程里面有2个main函数，此时处理方法为：
在ffmpeg.h中添加一个函数声明：
int ffmpeg_main(int argc, char **argv);
在ffmpeg.c中找到main函数，把main函数改为ffmpeg_main。
    - 调用命令行范例
 
        添加头文件：#import "ffmpeg.h"
        调用命令行
            #import "ffmpeg.h"
            
            int numberOfArgs = 16;
            char** arguments = calloc(numberOfArgs, sizeof(char*));
             
            arguments[0] = "ffmpeg";
            arguments[1] = "-i";
            arguments[2] = inputPath;
            arguments[3] = "-ss";
            arguments[4] = "0";
            arguments[5] = "-t";
            arguments[6] = durationChar;
            arguments[7] = "-vcodec";
            arguments[8] = "copy";
            arguments[9] = "-acodec";
            arguments[10] = "aac";
            arguments[11] = "-strict";
            arguments[12] = "-2";
            arguments[13] = "-b:a";
            arguments[14] = "32k";
            arguments[15] = outputPath;
                
            int result = ffmpeg_main(numberOfArgs, arguments);
            
>其中inputpath和outputpath是文件路径。经测试，**这两个路径不支持asset-library://协议和file:// 协议，所以如果是要用相册的文件，我目前的解决办法是把它拷贝到沙盒里面。**

4. 改关闭进程为关闭线程
如果顺利进行到了第5步，在app中是能够用命令行处理视频了，但会出现一个问题，app会退出。经肖大神提醒，发现了命令行执行完毕之后会退出进程。而iOS下只能启动一个进程，因此必须改关闭进程为关闭线程，或者直接把关闭进程的方法给注掉。
在ffmpeg.c中可以看到，执行退出进程的方法是exit_program，定位到了cmdutils.c中执行了c语言的exit方法。这里我将它改为了pthread_exit（需要添加#include 头文件）。在xcode项目中使用时，则可以用NSThread来新开一个线程，执行完毕之后，把线程关闭了即可。再使用NSThreadWillExitNotification通知，即可监听线程退出的情况。

5. 修复ffmpeg.c里面的一个bug
在实际项目中，可能需要多次调用命令行，但在多次调用命令行的过程中，发现ffmpeg.c的代码中会访问空属性导致程序崩溃。逐步debug后发现，很多指针已经置空了，但它们的计数却没有置零，不知道是不是ffmpeg.c的一个bug。修复方法如下：在ffmpeg_cleanup方法下，将各个计数器置零，包括：
nb_filtergraphs
nb_output_files
nb_output_streams
nb_input_files
nb_input_streams
置零之后，重复使用ffmpeg_main方法一切正常。