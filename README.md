## Jquery Ajax Progresss

A simple patch to jQuery that will call a 'progress' callback, using the 
[XHR.onProgress](https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest/Using_XMLHttpRequest#Monitoring_progress) event

###Composer
install: **composer require ilopx/jquery-ajax-progress**

### Usage

Simply include the script on your page:

```html
<script src="js/jquery.ajax-progress.js"></script>
```

Then, whenever you make an ajax request, just specify a progress callback:

```javascript
$(function() {
    function setProgress(loaded, total) {
        var precis = (loaded / total) * 100;
        precis = precis.toPrecision(3)+'%';
        $('#progress').css({
            width: precis
        }).html('<span>'+precis+'</span>');
    }

    $.ajax({
        method: 'GET',
        url: 'server.php',
        data: {
            action: true
        },
        success: function() {
            alert('YAYE!');
        },
        error: function() {
            alert('AWWW!');
        },
        progress: function(e) {
            if(e.lengthComputable) {
                setProgress(e.loaded, e.total);
                $('#content').html(e.srcElement.responseText);
            }
            else {
                console.warn('Content Length not reported!');
            }
        }
    });
});
```

You can see it in action on the `example/index.php` page.

### Notes

 - This will not work using the `file://` protocol, see [XMLHttpRequest - Monitoring Progress](https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest/Using_XMLHttpRequest#Monitoring_progress) for more info.