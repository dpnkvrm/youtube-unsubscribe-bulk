### Quick Links

*   [How to Manually Unsubscribe YouTube Channels in Bulk](#how-to-manually-unsubscribe-youtube-channels-in-bulk)
    

*   [How to Automatically Unsubscribe YouTube Channels in Bulk](#how-to-automatically-unsubscribe-youtube-channels-in-bulk)
    

### Key Takeaways

*   To manually unsubscribe from a YouTube channel, click on Subscriptions > Manage > Subscribed > Unsubscribe.
*   You can also use a script to automate the unsubscription process, which allows you to unsubscribe from every channel almost instantaneously.

YouTube doesn't offer a built-in feature to unsubscribe from all channels in one go. Nevertheless, there are some workarounds you can use. This guide will show you how to manually unsubscribe from YouTube channels in bulk or use a custom script in Inspect Element to automate this process.

How to Manually Unsubscribe YouTube Channels in Bulk
----------------------------------------------------

For bulk [unsubscribing from YouTube channels](https://www.howtogeek.com/677781/how-to-unsubscribe-from-a-youtube-channel/), open YouTube in your browser, click on the "Subscriptions" tab on the left, and then click "Manage" in the top-right.

![Opening the subscriptions page listing all subscribed YouTube channels.](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/1-opening-the-subscriptions-page-listing-all-subscribed-youtube-channels.jpg)

This will lead you to a page listing all your subscribed channels. Click the "Subscribed" button next to each channel and click "Unsubscribe."

![Manually unsubscribing a YouTube channel on the web.](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/2-manually-unsubscribing-a-youtube-channel-on-the-web.jpg)

If you have only a few YouTube channels, you can manually unsubscribe from them. However, the task becomes tedious if you want to unsubscribe too many channels. For such cases, you can use a custom script to automate this process.

How to Automatically Unsubscribe YouTube Channels in Bulk
---------------------------------------------------------

Go to the page that displays all the channels you are subscribed to, right-click anywhere here, and select "Inspect" to [open the HTML source](https://www.howtogeek.com/416108/how-to-view-the-html-source-in-google-chrome/) of YouTube.

[Opening the Inspect Element in Chrome.](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/3-opening-the-inspect-element-in-chrome.jpg)

Here, go to the "Console" tab. Next, copy the code provided at the end of the article. Then, return to the Inspect Element Console and paste the copied code on a new line.

![Opening the Console tab in Inspect Element to paste the code.](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/4-opening-the-console-tab-in-inspect-element-to-paste-the-code.jpg)

If you encounter a warning, as shown in the image below, simply type **allow pasting** on the following line and try pasting the code below it. Then, press Enter.

![Inspect Element showing warning about not pasting custom code in the source.](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/inspect-element-showing-warning-about-not-pasting-custom-code-in-the-source.jpg)

The script will initiate the process and unsubscribe each YouTube channel sequentially. Allow it to run until it unsubscribes from all your YouTube channels. During this process, it will notify you about the number of channels unsubscribed and the ones remaining.

![Custom script automatically unsubscribing YouTube channels in bulk and notifying about remaining channels left.](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2023/12/5-custom-script-automatically-unsubscribing-youtube-channels-in-bulk-and-notifying-about-remaining-channels-left.jpg)

If the script gets stuck at any point and stops unsubscribing channels, refresh the page and repeat the process to run the script from the beginning.

```
/**
* YouTube bulk unsubscribe fn.
* Wrapping this in an IIFE for browser compatibility.
*/
(async function iife() {
// This is the time delay after which the "unsubscribe" button is "clicked"; Change it as per your need!
var UNSUBSCRIBE_DELAY_TIME = 2000
/**
* Delay runner. Wraps `setTimeout` so it can be `await`ed on.
* @param {Function} fn
* @param {number} delay
*/
var runAfterDelay = (fn, delay) => new Promise((resolve, reject) => {
setTimeout(() => {
fn()
resolve()
}, delay)
})
// Get the channel list; this can be considered a row in the page.
var channels = Array.from(document.getElementsByTagName(`ytd-channel-renderer`))
console.log(`${channels.length} channels found.`)
var ctr = 0
for (const channel of channels) {
// Get the subscribe button and trigger a "click"
channel.querySelector(`[aria-label^='Unsubscribe from']`).click()
await runAfterDelay(() => {
// Get the dialog container...
document.getElementsByTagName(`yt-confirm-dialog-renderer`)[0]
// and find the confirm button...
.querySelector(`[aria-label^='Unsubscribe']`).click()
console.log(`Unsubsribed ${ctr + 1}/${channels.length}`)
ctr++
}, UNSUBSCRIBE_DELAY_TIME)
}
})()
```
    

* * *

That's how you can automatically unsubscribe from all YouTube channels in bulk and build a clean subscription list from scratch.
