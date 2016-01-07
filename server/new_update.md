###Server:
1. parepared-Images:
  - id
  - category
  - prepared_image

2. user-Images:
  - id 


###Client db:
1. prepared_image:
  - id
  - prepared_image
  - category

2. prepared_voice:
  - id
  - prepared_bg_voice_namel
  - category
  - time

###ios dir structure
1. Documents
    + tmp
       - tmp1
    + Resource
       - Images
       - bg_music

### Flow
	new = [self QueryNewFromServer]
	info.list.updated = False;
	if new:
	    download new staff
	    update local resource 
	    update database
	
	choosePhotoController
	if info.list.updated = True:
	  query database and store list, set info.list.updated = True;
	normal show 