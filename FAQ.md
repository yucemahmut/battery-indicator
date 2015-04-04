<h1>Frequently Asked Questions</h1>

If you have a question or a problem, please look over the titles of
the questions here to see if it has already been addressed. The last
question explains what to do if it has not.

<h2>Contents</h2>


## I'm having a problem with purchasing, downloading, or installing the app. ##

If you are having problems with purchasing, downloading, or installing the app, please see [Google's support page](http://support.google.com/googleplay/bin/answer.py?hl=en&answer=1067233).  I can only help with troubleshooting the app itself.

## What's new in the update? ##

Please see the [Changelog](http://code.google.com/p/battery-indicator/wiki/Changelog?tm=6).

## I don't have access to the Android Market. Can I download the APK? ##

I'm glad to finally have the APKs available for download from this
site.  Please see
[Package Download / Donation Form](http://code.google.com/p/battery-indicator/wiki/PackageDownload?tm=2)
or just click on the "Downloads" tab at the top of this page.

## Why does the app ask for the permissions that it does? ##

That's a very good question, one that I think every developer should
explicitly address. One thing to keep in mind is that permissions are
all-or-nothing and happen at install time: if some optional feature
requires a permission in order to work, I need the app to request it
at install time.  By explaining here exactly why the app requests its
permissions and how it uses them, and of course by sharing the source
code publicly, I hope to make it an easy decision to grant the
permissions and install the app.

**WRITE\_EXTERNAL\_STORAGE ("modify/delete SD card contents")**

This is so that the log viewer can export the logs to CSV on the SD
card.  Nothing else is ever written to storage, so if you don't use
the optional log feature, or if you never export the logs to CSV, then
the app will never touch the SD card.

**WAKE\_LOCK ("prevent phone from sleeping")**

This is necessary for the lock screen functionality to work properly.
It won't be used at all if you don't have the app set to automatically
disable and reenable the lock screen. If you do have that turned on,
then the wake lock is released immediately after acquisition. It's
just a work-around to make sure the the lock screen is reenabled
properly; when I say immediately, I mean _immediately_: http://code.google.com/p/battery-indicator/source/browse/trunk/src/com/darshancomputing/BatteryIndicatorPro/BatteryIndicatorService.java?r=258#213

**RECEIVE\_BOOT\_COMPLETED ("automatically start at boot")**

This is so the app can optionally start itself on device startup
(which most users seem to want.)

**DISABLE\_KEYGUARD ("disable keylock")**

This is for the optional lock screen functionality.

**VIBRATE ("control vibrator")**

This if for the optional alarms, so that they can optionally vibrate
when they are triggered.

## How does Battery Indicator work? / How does it know what percentage the battery is at? ##

The Android system has a mechanism where you can register your app to
be notified when the battery status changes.  So Battery Indicator has
a background service that is technically always running but is
essentially always sleeping and using basically no system resources
(because it doesn't do any polling -- it just sleeps and waits to be
notified by the OS when something changes), then when the battery
charge (or plugged-in status) changes, the system wakes it up and
tells it what the new battery charge (and status) is.  Then it takes
just a few milliseconds to change its icon to reflect the current
charge (and status) and goes back to sleep.

If you're technically oriented, the documentation for the core API
that Battery Indicator uses is here:
http://developer.android.com/reference/android/os/BatteryManager.html
and the actual source code to the development version of the app is
here: http://code.google.com/p/battery-indicator/source/browse/trunk .

## Will using Battery Indicator affect my battery life? ##

No.  Because Battery Indicator doesn't do any polling, it is virtually
always asleep, not using any CPU (and very little RAM).  When the
battery indicates to the system that its charge or state has changed,
the system then wakes Battery Indicator, which quickly updates itself
and goes back to sleep.

Because of this, Battery Indicator will have no measurable or
perceptible effect on battery life.

## Your app only displays the charge with a 10% resolution.  Please fix this. ##

~~Battery Indicator displays exactly what the system reports to it.  The
(original) **Motorola DROID**, the **Motorola Droid X**, the **Motorola
Droid 2**, and the **Samsung Moment** are known to only report their
charge at 10% intervals.  This is dumb but not my fault.  I've heard
from one user that the **Motorola i1** actually only reports at 100%,
65%, and 35%.  That's terrible, but again, there's nothing I can do
about it if that's how Motorola designed their device.  If you have
one of these crippled devices, and you find it bothersome, I think it
might be helpful to let your carrier and/or device manufacturer know
how you feel.  Motorola in particular is building a bad reputation in
this regard, so it would be good for them to know that people care
about the issue.~~

~~All other Android devices, as far as I know, report their charge at 1%
intervals.  _If you have a device that fails to do so (other than those
already mentioned in the previous paragraph), please let me know, and
I'll add it to this list_.~~

_**Update**_: Please see
[OnePercentHack](http://code.google.com/p/battery-indicator/wiki/OnePercentHack?tm=6).

## Battery Indicator is inaccurate or jumps by several percent. ##

If it always jumps by exactly 10%, please see the previous question.

Using Battery Indicator in a supported software environment, you may
sometimes see the indicator jump by 2 or 3 percent during heavy
multitasking on older phones.  This is a design feature of Android,
and it's probably exactly what you want.  Battery Indicator uses very
little RAM, but it does use _some_.  If you are heavily multitasking
on an older device (with little RAM), Android will temporarily kill
background services (including Battery Indicator's) to reclaim
whatever little bit of RAM it can find.  Once things settle down, it
will restart the Battery Indicator background service, which will then
update the icon to the current level, which may be a few percent away.

The other thing that can cause this type of behavior, however, is
using it in an unsupported software environment:

  * **Third-Party ROMs**

These often work perfectly fine with Battery Indicator, but they
sometimes fail miserably when it comes time to restart background
services.  They can also by overzealous about killing them off.

  * **"Task Killer" style apps**

These apps cause way more problems than they solve and have a tendency
totally mess up any apps that legitimately run background services, as
Battery Indicator does.  Needless to say, if you're running an app
that constantly kills off Battery Indicator, there's nothing I can do
to make Battery Indicator work in such an environment.

## Battery Indicator seems to slow my phone down. ##

The only time I've ever heard of anything like that is when people use
a "task killer" type of app.  Those apps really wreak havoc on apps
like mine that use a background service; I recommend uninstalling any
apps like that that you might have.  Some people have gotten things to
work by telling the task killer to "ignore" my app, but most haven't
been able to get things to work until they uninstalled the task
killer.

## The indicator disappears from the status bar. ##

You're using some kind of "task killer" app, which is killing Battery
Indicator.  See the previous two questions.

## Can I have just the icon without the notification? / <br> Can I have just the notification without the icon?</h2>

~~Unfortunately, that's not possible at this time.  In all current
versions of Android, the notification icon and the actual notification
(in the pull-down menu) are tied together. If you try to set a
notification without an icon, the notification doesn't show up, and
the only way to get an icon in the status bar is to set a
notification.  If a future version of Android allows it, I will
definitely add an option to allow people to customize things in this
way.~~

On newer devices (Jellybean / Android 4.1+), the Pro version allows
you to hide the status bar icon while leaving the notification in the
tray by setting the notification priority to "Minimum".

You can also now hide _both_ the status bar icon _and_ notification
_together_ in both the free and Pro versions be selecting "Hide
notification" from the menu.  This is primarily intended for users who
prefer to use the widget.

There's still no way to have the status bar icon but _not_ the
notification in the tray.

## Can you make your app replace the stock battery icon? ##

Unfortunately, that's not possible at this time.  If a future version
of Android allows it, I will definitely do so.

## Can you add more color options for the icon? / <br> Can you make the icon look different when the battery is charging? / <br> Why can't I have my icon turn red above 30% or amber above 50%?</h2>

The way Android currently implements notifications is that any icons<br>
you want to put in the status bar <i>must</i> be included in the app.  You<br>
can generate images on the fly in an Android application, and you can<br>
use these images in most parts of your application, but you cannot use<br>
generated images in the status bar.<br>
<br>
What this means for Battery Indicator is that I can't make the icon as<br>
customizable as I would like to.  Since I need to include both high<br>
and medium resolution sets of images for every option (grayscale, red,<br>
amber, and green), the app is already over half a megabyte in<br>
size. While it would be easy to add more icons with different colors<br>
and styles, or add a lightning bolt to indicate charging, for example,<br>
this quickly increases the size of the package.  (A bigger package<br>
takes up storage space on people's phones and -- even worse -- uses up<br>
RAM that is generally in short supply.)<br>
<br>
Hopefully a future version of Android will allow developers to use<br>
dynamically-generated images in the status bar.  If that happens, then<br>
I will be able to make the icon for Battery Indicator much more<br>
configurable without having to worry about the package size. In the<br>
mean time, I'll continue to do my best to balance usefulness and<br>
configurability on the one hand and keeping the app very lightweight<br>
on the other.<br>
<br>
<h2>Please allow the app to be installed to SD card for Android 2.2+.</h2>

I hear you.  However, Google is very clear that apps such as Battery<br>
Indicator <b>should not</b> allow this<sup>1</sup>.  If I did this, and you moved<br>
Battery Indicator to your SD card, then whenever you unmounted the<br>
card from your phone (by plugging your phone into your computer to<br>
copy files to/from your phone, for example), Battery Indicator would<br>
be closed and would not be restarted until you did so manually.<br>
<br>
Even if you're sure that you almost never mount your phone as a USB<br>
mass storage device and/or that you don't mind this behavior, there's<br>
no doubt that enabling it would confuse many of my users and this in<br>
turn would cause a huge headache for me.  Battery Indicator simply<br>
isn't one of the apps where this should be enabled.<br>
<br>
<code>[1]</code> <a href='http://developer.android.com/guide/appendix/install-location.html#ShouldNot'>http://developer.android.com/guide/appendix/install-location.html#ShouldNot</a>