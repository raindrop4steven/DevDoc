## UIImagePickerDelegate

#### Could get edited image and original image

	- (IBAction)takePicture:(id)sender {
		UIImagePickerController *imagePicker = [[UIImagePickerController alloc] init];
		if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
			[imagePicker setSourceType:UIImagePickerControllerSourceTypeCamera];
		} else {
			[imagePicker setSourceType:UIImagePickerControllerSourceTypePhotoLibrary];
		}

		[imagePicker setAllowsEditing:YES];
		[imagePicker setDelegate:self];

		//place image picker on the screen
		[self presentViewController:imagePicker animated:YES completion:nil];
	}

	- (void)imagePickerController:(UIImagePickerController *)picker 
	didFinishPickingMediaWithInfo:(NSDictionary *)info {

		[picker dismissModalViewControllerAnimated:YES];
		[picker release];

		// Edited image works great (if you allowed editing)
		myUIImageView.image = [info objectForKey:UIImagePickerControllerEditedImage];
		// AND the original image works great
		myUIImageView.image = [info objectForKey:UIImagePickerControllerOriginalImage];
		// AND do whatever you want with it, (NSDictionary *)info is fine now
		UIImage *myImage = [info objectForKey:UIImagePickerControllerEditedImage];
	}


