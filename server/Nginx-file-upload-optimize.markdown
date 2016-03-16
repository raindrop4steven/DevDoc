## Nginx upload optimize

#### OPTIMIZED FILE UPLOADING WITH PHP & NGINX

Performance is often important to people using nginx – and for good reason, of course. Sadly, while many people will optimize their software stack they will rarely work on optimizing the back-end code; and even more rarely will they eliminate single points of failure. Such was also the case when SitePoint recently published an article about uploading large files with PHP. This post will discuss a method to accept uploads that will scale far better and not offer malicious users an easy DoS vector.

#### The Problem

File uploads are used in many places, depending on your site people might be adding avatars, personal pictures, music or any other type of file. The size of uploads can vary a lot but in the end it doesn’t really matter much, you’re still offering a malicious user a single point of failure where he can direct his denial of service attack.

Allow me to illustrate. Lets say you have an upload form for people to upload pictures, you run Apache in pre-fork mode with mod_php and 50 max children, otherwise known as the standard Apache setup.

Each time Apache accepts an upload one of these processes is going to be busy for the duration of the upload. Do you see the problem here? File uploads to PHP are essentially really long-running scripts and you’re going to run out of Apache processes quickly. It might not even be a malicious user, you could be a victim of your own popularity.

If you’re using nginx then you’re already better off as nginx will buffer the file upload to disk and only pass it to your fastcgi back-end once the file upload is complete. If you’re uploading a 1 GB file nginx is still going to send 1 GB of data over fastcgi, though, but we can do something about that.


#### The Solution

Thankfully we are not without options and developing a scalable system for uploading files is not too difficult. To help us out we’ll use two third-party nginx modules – namely the upload module and upload progress module.

The upload module will handle the actual upload for us in nginx and when complete will pass the path of the file to PHP for us to know where the file is. This way PHP will not be waiting on the data to be sent but only decide what to do with the data once it’s on the server, this means PHP-wise your file upload can have a sub-second execution time, and at this point your bottleneck is going to be either your network or disk IO!

The upload progress module is fairly self-descriptive in that it will monitor and report the progress of uploads. It accepts a unique identifier with the form submission and when given this identifier will report the status of the upload. Simple.

#### The Execution

If you’ve never compiled nginx with a third-party module then you’ll be happy to know that it’s fairly simple. Download the source code, extract it and add the following configure option.
    
    --add-module=/path/to/nginx-upload-module
    --add-module=/path/to/nginx-upload-progress-module
    make, make install and you’re ready to configure it.

The configuration is a bit more complex and might seem overwhelming at first, but it is fairly easy to comprehend given a few seconds thought.

    http {
      upload_progress uploads 5m;
     
      server {
        # This just rewrites all requests to a front-controller for SEF URLs.
        location @frontcontroller {
          rewrite ^ /index.php last;
        }
     
        location = /progress {
          report_uploads uploads;
        }
     
        location /upload {
          # Pass altered request body to this location
          upload_pass   @frontcontroller;
     
          # Store files to this directory
          # The directory is hashed, subdirectories 0 1 2 3 4 5 6 7 8 9 should exist
          upload_store /var/tmp/fuploads 1;
     
          # Set the desired user permissions
          upload_store_access user:r group:r all:r;
     
          # Set specified fields in request body
          upload_set_form_field $upload_field_name.name "$upload_file_name";
          upload_set_form_field $upload_field_name.path "$upload_tmp_path";
     
          # Inform backend about hash and size of a file
          upload_aggregate_form_field $upload_field_name.sha1 "$upload_file_sha1";
          upload_aggregate_form_field $upload_field_name.size "$upload_file_size";
     
          # This directive specifies any extra POST fields which should be passed along.
          #upload_pass_form_field "^usession$";
     
          upload_cleanup 400 404 499 500-505;
     
          track_uploads uploads 5s;
        }
      }
    }
All the directives are documented in the nginx wiki or on the module download page so I’m not going to go into too much detail about the configuration. The configuration here is stripped down and missing essential non-related directives but lets have a look at the important parts.

In the http block we allocate a memory buffer for the upload progress module to use for tracking, it does not need to be very large as it doesn’t store overly much info per upload, the 5 MB I have assigned it is probably overkill even though it’s used in a system handling many uploads.

The /progress location is defined as the URI we’ll use for reporting uploads tracked in the uploads buffer we defined earlier. At the very bottom of the configuration you can see that we have set the location /upload as the location for tracking uploads.

Conveniently, this is also the location that will handle the upload! In short, uploads are stored in /var/tmp/fuploads/x where x is between 0 and 9. Once done the module will pass it to the @frontcontroller named location which basically just rewrite the request to a PHP file. This will be the file that will handle the PHP end of the file upload. In this example my index.php file would have /upload/ as request URI and route the request the request to the proper controller, but how you handle it doesn’t really matter.

### Putting It All Together

Right now you actually have a working setup. File uploads POSTed to /uploads will be handled and tracked by nginx so now it’s time to put this data to use by displaying a nice progress bar to the user. For this we will create a javascript-based uploader, it will degrade gracefully in case javsacript isn’t enabled, but in that case won’t support displaying a progress bar.

    <form id="javascript-upload" action="/upload/" enctype="multipart/form-data" method="post">
      <label for="jfile">File Upload:
        <input id="jfile" name="file" type="file" />
      </label>
      <input type="submit" value="Upload File" />
    </form>
    <div style="border: 1px solid black; width: 300px;">
      <div id="status" style="background-color: #D3DCE3; width: 0px; height: 12px; margin: 1px;"></div>
    </div>
    <div>
      <span id="received"> </span>
      <span id="speed"> </span>
    </div>
This is a fairly standard upload form. In addition we’ve defined a div for a progress bar and a few spans for information about received data and the upload speed. Now let’s have a look at the javascript required, this example uses MooTools but it’s much the same concept in native javascript, jquery or whatever you prefer.

    $('javascript-upload').addEvent('submit', function(e) { // On submit of upload form.
      var received = 0;
      var percent  = 0.0;
      var perform;
      var periodical;
      var uuid = Math.floor(Math.random() * 16).toString(16); // Unique uploader ID
      var check = 2000; // Milliseconds between each XHR request.
     
      $('javascript-upload').action += '?X-Progress-ID=' + uuid; // Assign ID to upload.
     
      var request = new Request({ // Define the XHR request.
        url: '/progress?X-Progress-ID=' + uuid, // Using same identifier!
        method: 'get',
        link: 'cancel',
        onComplete: function(response) {
          var json = JSON.decode(response);
          if (json.state == 'uploading') {
            var delta = json.received - received;
            var bytes = delta / (check / 1000);
            received  = json.received;
            percent   = (json.received / json.size) * 100;
     
            $('status').tween('width', 298 * percent / 100);
            $('received').innerHTML = 'Received ' + Math.round(json.received / 1024) + '/' + Math.round(json.size / 1024) + ' KB';
            $('speed').innerHTML    = 'Speed ' + Math.round(bytes / 1024) + ' KB/s';
     
            if (percent >= 100) {
              $clear(periodical); // Upload done, stop polling Nginx.
            }
          }
        }
      });
     
      perform = function () {
        request.send();
      }
     
      periodical = perform.periodical(check);
    });
I did my best to put in the required comments to make it understandable. But in short what we do is capture the submit event of the upload form and inject our own javascript code. It’s important to note that we do not return false or prevent the upload from taking place. We then define an XHR request to the /progress URI we configured earlier and provide it with the unique upload identifier. It will return data in JSON format which we can then parse and use to calculate how progress and upload speed. The 298 in the tween method call is the width of the progress bar (300) minus the margins (1 each).

So there you have it, scalable file uploading that won’t kill your back-end.

### Drawbacks

Sadly, nothing is ever completely perfect. While the method the upload module uses by passing PHP the path to the uploaded file instead of the actual file data is much faster and a smarter concept, it does mean that we cannot use the standard back-end code. There will be no $_FILES array for us to use but rather we’ll get the data in $_POST. Using nginx by itself will make it scalable enough while providing the $_FILES array, but if you’re writing a custom application then the upload module can come in handy.

Last updated:Friday, February 8, 2013

### Link
[file-uploading-with-php-and-nginx](https://blog.martinfjordvald.com/2010/08/file-uploading-with-php-and-nginx/)