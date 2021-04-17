# WM01
## BRIEFING
View the page at https://cfta-wm01.allyourbases.co and try to get the flag.

## Solution

If we view the source code of the site we can see there is a javascript file located at /assets/js/site.js. The file looks like this:

```js
// Image slideshow. Moved in from /new-images when each is finalised
function displayImg(num) {
    let nextNum = num + 1;
    if (nextNum > 6) {
        nextNum = 1;
    }
    document.getElementById("img_" + num).style.opacity = "1";
    setTimeout(function() {
        setTimeout(function() {
            document.getElementById("img_" + num).style.opacity = "0";
        }, 1000);
        displayImg(nextNum);
    }, 5000);
}

window.onload = function() {
    setTimeout(function() {
        displayImg(1);
    }, 500);
};
```

The important thing to notice is the `/new-images` directory. If we navigate there we find 6 photos.![Uploading portal.png…]()
