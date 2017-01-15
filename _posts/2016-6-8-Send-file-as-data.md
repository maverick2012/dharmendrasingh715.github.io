---
layout: post
title: Sending file as data in jquery ajax
---

Recently I was working on a project where I needed to upload a file in a single page application, so form submission to a backend file was out of the question.

![_config.yml]({{ site.baseurl }}/images/whatif.jpg)

There is also option of uploading files directly with ajax but the backend guy asked me if I can send files in _Base64_ format. 

I knew I can convert image into _Base64_ by loading it in canvas this way.
 
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