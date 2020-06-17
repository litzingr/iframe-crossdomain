iframe-crossdomain
======
**iframe crossdomain** is a pair of short javascript scripts to capture personally owned html pages and transmit information to affect the iframe (changing height is our example).


**inside iframe**
```
function sendHeight() {
  if (parent.postMessage) {
    var height = document.getElementById('pageDiv').offsetHeight;
    parent.postMessage(height, '*');
  }
}
setInterval(sendHeight, 1000);
```

This is the script that sends the height from the page within the iframe. (This means you need access to the page to be able to run this script). You cannot change the iframe's styling from here.

**outside iframe**

```
var eventMethod = window.addEventListener ? "addEventListener" : "attachEvent";
var eventer = window[eventMethod];
var messageEvent = eventMethod == "attachEvent" ? "onmessage" : "message";

eventer(messageEvent, function(e) {
    if (isNaN(e.data)) return;
    document.getElementById('iframeID').style.height = e.data + 'px';
}, false);
```
This is the script that receives the height from the page within the iframe. This should be placed near the iframe in the html for readability. This is what can affect the iframe's styling.

## Version 1
