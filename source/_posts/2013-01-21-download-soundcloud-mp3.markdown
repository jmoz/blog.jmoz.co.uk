---
layout: post
title: "Your files aren't safe on Soundcloud"
date: 2013-01-21 12:00
comments: true
categories: [soundcloud, download, chrome]
published: false
---
I use Soundcloud to listen to and download free mixes from DJs and events I'm into.  Recently I've noticed a trend where artists are putting up their EPs and tracks in full form as a preview, and provide their own download links where the user can buy the track and have the Soundcloud download disabled.  In theory this should provide a user with a taster, then they'll head to the itunes link and pay for the download.

You can circumvent the missing/disabled Soundcloud download feature and **download Soundcloud for free**.

If you are on one of the pages where the Soundcloud download is not available, this means you will have access to a 128kbps mp3 file.  If you're on one of the pages where the Soundcloud download *is* available, then use that as the downloadable file is usually a higher quality than the hacked method.

For example, let's take a freely available [Jackmaster Boiler Room mix](https://soundcloud.com/platform/jackmaster-b2b-loefah-60-min).

First, you will need Google Chrome.  Load the page.  Press CMD+ALT+I (or probably CTRL+ALT+I on Windows) to load the developer tools bar.  Click the tab 'Network', then at the bottom of the screen click 'XHR'.  At the bottom left of the screen, to the right of the magnifying glass, click the icon that says 'Use large resource rows'.  Now that you've got your toolbar set up and the page has loaded all it's assets, click the icon at the bottom left of the screen that clears the logged data.

Now click the Soundcloud play button, you should see some requests logged in the toolbar.

{% img http://i.imgur.com/TP4rnqQ.png %}

Look at the 3rd file in that screenshot.  You can see the type is audio/mpeg and the file name ends with *128.mp3*.  Obviously, this is the 128kbps mp3 file.

To download the file, the easiest way is to open it in a new tab then save the file.  First, right click the *128.mp3* file and you'll see 'Open Link in New Tab', click this.

{% img http://i.imgur.com/Q2TYTW0.png %}

Next a new black tab will open and the mp3 should start playing.  Now go to File > Save Page As.. and there's your mp3.

{% img http://i.imgur.com/iGsn6CI.png %}

Interestingly enough, if you look at the console tab in your developer toolbar, you'll see a message from Soundcloud ;)

{% img http://i.imgur.com/gJtK5pN.png %}

For this example, a download link is already available so you don't need to use this method.  The 128kbps file this method produces is around 55Mb whereas the builtin Soundcloud downloaded file is a higher quality and weighs in at 140Mb.

All of these steps also apply to Soundcloud pages where there is no free download link.  This means if you upload some new tracks you think are private, unfortunately a user can still download them for free.

Producers and DJs, *your files aren't safe on Soundcloud*.

Ps. I've looked a little bit into their app and all of this can be achieved programmatically.  Oh and this method also applies to Mixcloud.