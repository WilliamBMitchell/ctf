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

The important thing to notice is the `/new-images` directory. If we navigate there we find 6 photos. The below photo tells us where to go next:

![miihome](miihome.jpg =250x)

If we navigate to `mii-home` we find a login portal. The portal requires an email and a password. We have the email `rupert@get-vizual.med.ia` from the bottome of the home page.

For the password, we take a look at the source code:

```html
        <div class="content">
            <div class="portfolio">
                <img src="assets/images/IMG_20201123_0945.jpg" alt="Street scene" id="img_1" style="opacity: 0"><br>
                <img src="assets/images/IMG_20201217_1537.jpg" alt="Buildings" id="img_2" style="opacity: 0"><br>
                <img src="assets/images/IMG_20201203_1917.jpg" alt="Salad" id="img_3" style="opacity: 0"><br>
                <img src="assets/images/IMG_20201211_1642.jpg" alt="My office, guess my fav city!" id="img_4" style="opacity: 0"><br>
                <img src="assets/images/IMG_20201217_1428.jpg" alt="Car" id="img_5" style="opacity: 0"><br>
                <img src="assets/images/IMG_20201217_1629.jpg" alt="Fashion" id="img_6" style="opacity: 0"><br>
            </div>
```
The text `My office, guess my fav city!` stands out. In another photo there is a pillow with `NEW YORK` written on it:

![pillow](pillow.jpg =250x)


