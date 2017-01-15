---
layout: post
title: Sending file as data in jquery ajax
---

Recently I was working on a project where I needed to upload a file in a single page application, so form submission to a backend file was out of the question.

![_config.yml]({{ site.baseurl }}/images/whatif.jpg)

There is also option of uploading files directly with ajax but the backend guy asked me if I can send files in _Base64_ format. 

I knew I can convert image into _Base64_ by loading it in _canvas_ this way.

Read more on [_canvas_ .](https://developer.mozilla.org/en/docs/Web/API/FileReader)
 
 {% highlight javascript %}
    var img = new Image();
    img.src = src; //image to be converted
    var canvas = document.createElement('CANVAS');
    var ctx = canvas.getContext('2d');
    var dataURL;
    canvas.height = img.height;
    canvas.width = img.width;
    ctx.drawImage(img, 0, 0);
    dataURL = canvas.toDataURL();
    console.log(dataUrl); //log Base64 image string
 {% endhighlight %} 

 But the issue here is that image takes some time to load, resulting in the wrong Base64 string. Fortunately, we have an event for image load completion which we can use to ensure that our image has completely loaded before we draw it into _canvas_.
 
 The changed code.
 
 {% highlight javascript %}
    var img = new Image();
    img.onload = function() {
        var canvas = document.createElement('CANVAS');
        var ctx = canvas.getContext('2d');
        var dataURL;
        canvas.height = this.height;
        canvas.width = this.width;
        ctx.drawImage(this, 0, 0);
        dataURL = canvas.toDataURL();
        console.log(dataURL);
    };
    img.src = src;
 {% endhighlight %} 	

Finally decided to convert the code into a generic function that can be invoked any time to do the conversion for us.

 {% highlight javascript %}
    function toDataUrl(src, callback) {
 	    var img = new Image();
 	    img.crossOrigin = 'Anonymous';
 	    img.onload = function() {
 		    var canvas = document.createElement('CANVAS');
 		    var ctx = canvas.getContext('2d');
 		    var dataURL;
 		    canvas.height = this.height;
 		    canvas.width = this.width;
 		    ctx.drawImage(this, 0, 0);
 		    dataURL = canvas.toDataURL();
 		    callback(dataURL);
 	    };
 	    
 	    img.src = 'crop.png';
 	    if (img.complete || img.complete === undefined) {
 		    img.src = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///ywAAAAAAQABAAACAUwAOw==";
 		    img.src = src;
 	    }
    } 
 {% endhighlight %} 	

We can use it this way.

 {% highlight javascript %}
    toDataUrl('crop.png', function(base64Img) {
 	    console.log(base64Img);
    });
 {% endhighlight %}
 	
Also I added a check in function to make sure the load event fires for cached images too.

 {% highlight javascript %}
    if (img.complete || img.complete === undefined) {
        img.src = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///ywAAAAAAQABAAACAUwAOw==";
        img.src = src;
    }
 {% endhighlight %}
 
Later I found out that we can also use _FileReader_ API to read the file and convert into the _Base64_ format.

Read more on [_FileReader_ API.](https://developer.mozilla.org/en/docs/Web/API/FileReader)
    
We can then send the image we converted in any ajax request.      