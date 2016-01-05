### Curl

#### 1. Upload file
You need to use the -F option:
	
	-F/--form <name=content> Specify HTTP multipart POST data (H)

Try this:

	curl \
	  -F "userid=1" \
	  -F "filecomment=This is an image file" \
	  -F "image=@/home/user1/Desktop/test.jpg" \
	  localhost/uploader.php