<h1>The One-Percent Hack</h1>

<h2>Contents</h2>


## Summary ##

Many (most?) Motorola devices only show the battery level as a
multiple of 10% by default.  ~~There is now an option to apply a
workaround.  It is in the "Other Settings" page of the Battery
Indicator Pro settings, at the bottom.  It has also been added the the
settings page of the Free version.~~ **Update, 04 Feb 2013:** The
one-percent hack is now used automatically on applicable devices.  A
few people have requested the ability to turn it off, so this option
is planned for a future version.

## Technical Details ##

For some strange reason, Motorola decided to only show the battery
level in 10-percent increments on most of their Android devices.
However, many of these devices have a file at
`/sys/class/power_supply/battery/charge_counter` that contains the
current charge level to the nearest single percent.  (My understanding
is that this is not what this file is supposed to be used for.
Non-crippled devices often don't have the file, and when they do, they
use it according to the linux documentation, which describes a
different purpose -- so in those cases the file does not contain the
battery level.)

I originally assumed that I would need to set up a timer to trigger
the Battery Indicator Service to wake up and check that file at
regular intervals.  Luckily (and **very** strangely), at least on the
Droid X, the standard ACTION\_BATTERY\_CHANGED Broadcast Intent is
actually sent upon every 1-percent change -- down to the temperature
and voltage details.  In other words, they still send the Broadcast
just as often as non-crippled devices, with everything except the
current actual charge level (the thing most people are most interested
in).  That's unexpected -- what's the point of not including the most
important info?  I'd assumed the idea was to save battery power by not
broadcasting as often, but these devices _do_ broadcast just as
often...  In any case, it appears that all the crippled devices work
this way, which keeps the workaround very simple.

## Device Status ##

This is a partial list of devices that only show multiples of 10% by
default, along with their status with the hack.  ~~If your device only
shows 10% increments and this option doesn't work, please add a
comment to [issue 54](https://code.google.com/p/battery-indicator/issues/detail?id=54) to let me know what you found.~~

I'm no longer looking for more reports of it successfully
working. It's assumed to work on all devices other than the original
Droid (**update 04 Feb 2013:** and now the Defy XT makes _two_ devices
it doesn't help on), so I only need to hear reports of it _failing_ to
work.

| **Device** | **Status** |
|:-----------|:-----------|
| Motorola Atrix | Reportedly **works perfectly** with the hack enabled. |
| Motorola Atrix 2 | Reportedly **works perfectly** with the hack enabled. |
| Motorola Charm | Reportedly **works perfectly** with the hack enabled. |
| Motorola Cliq 2 | Reportedly **works perfectly** with the hack enabled. |
| Motorola Defy | Reportedly **works perfectly** with the hack enabled. |
| Motorola Defy XT | The hack does **not** help. |
| Motorola Defy+ | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid (original) | The hack does **not** help. |
| Motorola Droid 2 | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid 3 | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid 4 | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid Bionic | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid Razr | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid Razr Maxx | Reportedly **works perfectly** with the hack enabled. |
| Motorola Droid X | _My test device_. **Works perfectly** with the hack enabled. |
| Motorola Droid X2 | Reportedly **works perfectly** with the hack enabled. |
| Motorola Electrify | Reportedly **works perfectly** with the hack enabled. |
| Motorola Flipout | Reportedly **works perfectly** with the hack enabled. |
| Motorola Photon 4G | Reportedly **works perfectly** with the hack enabled. |
| Samsung Moment | _Unknown_ |

`*` Note that the 'Droid' name is is often replaced with 'Milestone' in
many countries, but most likely these statuses apply to the Milestone
versions, too.

## Testing Instructions ##

**_Update_:** _The hack is now assumed to work on all Motorola devices
besides the original Droid, so please disregard this section unless
you notice a problem._

Please follow these instructions if your device isn't listed, or if it
needs its information updated.  Working or not, please comment on
[issue 54](https://code.google.com/p/battery-indicator/issues/detail?id=54) and include the full name of your device along with what you
found.

If the app can determine on its own that your device won't be able to
support the hack (e.g., the `charge_counter` file doesn't exist, as on
the original Droid), then the One-Percent Hack option will be disabled
(grayed-out, unable to be turned on).  In this case, you've already
reached the end of testing, but please let me know in [issue 54](https://code.google.com/p/battery-indicator/issues/detail?id=54).

If the option is not disabled (that is, if you are able to turn it
on), please turn it on.  Ideally, the app will immediately start
showing your battery level with one-percent increments.  (Keep in mind
that there's a one-in-ten chance that it won't change immediately --
if it says 70 without this option, it may well happen to be right at
70.  So keep an eye on it and see if it soon discharges to 69 or
charges to 71.)

It seems possible that some devices will show non-multiples of 10, but
will still jump by 10 percent.  E.g., jumping from 65 to 55 to 45 as
your device drains.  _This is probably the most important thing I need
to know_, because if some devices do that, it means the Broadcast
isn't being sent often enough, and I'll have to set up a suboption to
poll at regular intervals.  Because none of my test devices work this
way, _**I need to hear from you if you have a device that does**_.

So if it appears to work, please keep on eye on it for a few hours and
try to verify that it drops fairly regularly at (close to) single
percent increments while discharging (or rises that way while
charging).

Then, let me know in [issue 54](https://code.google.com/p/battery-indicator/issues/detail?id=54) what you found.  Thanks!