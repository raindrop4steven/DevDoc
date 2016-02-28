## vmware

1. 怎样有效Shrink（压缩）Guest系统为Linx/Unix的VMWare虚拟机VMDK文件
	
	虚拟机在使用过程中，虚拟磁盘的大小会不断变大。即使你删除了磁盘中的文件，虚拟磁盘的大小仍然不会缩小。VMWare在VMWare Tools中推出了Shrink这个功能。在安装VMWare Tools后，在没有Snapshot的情况下，在Guest操作系统为Windows的情况下，能有效缩小虚拟磁盘大小。但如果在Guest操作系统为Linux时，此方法效果就不好了，而且有些挂载点无法Shrink。

     VMWare还推出了vmware-vdiskmanager工具，也能Shrink虚拟磁盘。在Guest操作系统为Linux时，单独用此工具没有什么效果。需要先在Guest系统中把未使用的 空间清零，在使用vmware-vdiskmanager，效果比较好。可以通过以下步骤有些缩小虚拟磁盘。

    1、cat /dev/zero > zero.fill;sync;sleep 1;sync;rm -f zero.fill

         在Shell中运行以上命令，能对未使用空间清零。

    2、关闭Guest操作系统，进入VMWare安装目录运行：

         vmware-vdiskmanager.exe -k f:/vmware/Fedora11/Fedora11.vmdk

         就可以有效缩小虚拟磁盘的大小，基本达到你用了多少占用多少的效果。

    用此方法分别对Guest系统为Fedora11和OpenSolaris10的VMDK文件进行Shrink，效果明显。

2. [Mac开发环境配置](http://www.jianshu.com/p/77a4349bf67b)
3. [Cocoapods](http://code4app.com/article/cocoapods-install-usage)

