# lucy
```
copy Reddit image post urls to clipboard

c  y                   u l

  \  \              /  /

          l u c y
```

<br>

This bookmarklet is written for **Reddit**, to capture and reformat urls from image posts, these reformatted urls are then copied to the clipboard and a 1 second overlay appears to confirm that the bookmarklet executed.

After running the bookmarklet, use CTRL + V to paste the urls either in a comment, or anywhere else that you want.

Single image posts are just the reformatted url.

Gallery image posts will be formatted as a numbered list, with a number preceeding each url in the order that they appeared as images in the gallery.

<hr>

This is the current working bookmarklet:
```
javascript:"use strict";void function(){javascript:(function(){function a(a){var b=a.match(c);return b%3F"https://i.redd.it/"+b[1]:null}function b(a){var b=document.createElement("div");b.style.cssText="\nposition: fixed;\ntop: 0;\nleft: 0;\nwidth: 100%25;\nheight: 100%25;\nbackground-color: rgba(0,0,0,0.5);\ncolor: white;\ndisplay: flex;\njustify-content: center;\nalign-items: center;\nz-index: 9999;\n",b.textContent=a,document.body.appendChild(b),setTimeout(function(){return document.body.removeChild(b)},1e3)}var c=/-(\w+\.[^%3F]*)/,d=[],e=document.querySelector("[id=\"post-image\"]");if(e)d.push(a(e.src));else{var f=document.querySelectorAll("gallery-carousel ul li > img");f.forEach(function(b,c){var e=b.src||b.getAttribute("data-lazy-src");e%26%26d.push(c+1+". "+a(e))})}var g=d.join("\n");if(navigator.clipboard%26%26window.isSecureContext)navigator.clipboard.writeText(g).then(function(){b("%23 image url/s copied to clipboard")});else{var h=document.createElement("textarea");h.value=g,document.body.appendChild(h),h.select(),document.execCommand("copy"),document.body.removeChild(h),b("%23 image url/s copied to clipboard")}})()}();
```

<hr>

This is the source code that it was created from:
```javascript
javascript:(function() {

    const regex = /-(\w+\.[^?]*)/;

    function transform_url(input_url) {

        const match = input_url.match(regex);

        if (match) {

            return `https://i.redd.it/${match[1]}`;
        }

        return null;
    }

    var urls = [];

    var post_image = document.querySelector(`[id="post-image"]`);

    if (post_image) {

        urls.push(transform_url(post_image.src));
    }
    else {

        var gallery_carousel = document.querySelectorAll(`gallery-carousel ul li > img`);

        gallery_carousel.forEach((img, index) => {
            var src = img.src || img.getAttribute(`data-lazy-src`);
            if (src) {
                urls.push(`${index + 1}. ${transform_url(src)}`);
            }
        });
    }

    var copiedText = urls.join('\n');

    if (navigator.clipboard && window.isSecureContext) {

        navigator.clipboard.writeText(copiedText).then(() => {

            showOverlay("# image url/s copied to clipboard");
        });
    }
    else {

        var textarea = document.createElement("textarea");

        textarea.value = copiedText;

        document.body.appendChild(textarea);

        textarea.select();

        document.execCommand("copy");

        document.body.removeChild(textarea);

        showOverlay("# image url/s copied to clipboard");
    }

    function showOverlay(message) {

        var overlay = document.createElement("div");
        overlay.style.cssText = `
position: fixed;
top: 0;
left: 0;
width: 100%;
height: 100%;
background-color: rgba(0,0,0,0.5);
color: white;
display: flex;
justify-content: center;
align-items: center;
z-index: 9999;
`;
        overlay.textContent = message;

        document.body.appendChild(overlay);

        setTimeout(() => document.body.removeChild(overlay), 1e3);
    }
})();
```
