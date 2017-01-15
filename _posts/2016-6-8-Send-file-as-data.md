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
    
{% endhighlight %} 