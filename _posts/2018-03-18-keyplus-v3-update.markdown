---
layout: post
title:  "keyplus controller update"
date:   2018-03-18
categories: keyboards
---

{% include image.html url="/assests/imgs/first-prototypes/keyplus-controller-v3.jpg" description="The keyplus mini v3" %}

It has been a while but I've finally got my next batch of controllers ready. The
main change in this version is a new USB Type-C connector. This connector has
a couple advantages over the one I used previously:

* It doesn't have any SMD pads hidden underneath the connector. This makes it
  easier to inspect visually, and it is possible to handle solder if you really
  want to.
* It is easier to route without using fine tolerances.
* It extends about the same distance above and below the board. So it is good
  for low profile builds.

For more information about the controller check out [my previous post](/2017/09/17/keyplus-mini-controller-beta.html).

{% include image.html url="/assests/imgs/first-prototypes/keyplus-controller-v3-usb.png" description="New USB Type-C connector profile." %}

## Firmware updates

Besides bug fixes, a lot of the work I've been doing on the firmware has
been improving the python code for interfacing with keyplus keyboards. One
new feature is the ability to read the current layout that is loaded on
your keyboard.  I haven't got any UI to take advantage of this feature yet, but
I want to start working a GUI now so that you will be able edit your layouts
without any config file (in some cases you still might need/want one though).

I've also cleaned up the python API making it easier to use in scripts to send
commands to your keyboard from the host PC. With the new API you should be able
to do stuff like this:

{% highlight python %}
>>> import keyplus
>>> keyplus.find_devices()
[<keyplus.keyboard.KeyplusKeyboard object at 0x7f6353e22518>]
>>> kb = keyplus.find_devices()[0]
>>> kb.connect()
>>> kb.set_indicator_led(led_num=0, state=1)
{% endhighlight %}

Check out the [change log](https://github.com/ahtn/keyplus/blob/master/CHANGELOG.md#internal-changes) on github for a more comprehensive list of what has
been changed/added.

## Production

{% include image.html url="/assests/imgs/first-prototypes/keyplus-controller-v3-panel.jpg" description="Controllers on panel from the factory." %}

This was my first time getting PCBs assembled at the factory, so I was a little
bit nervous that something would go wrong, but everything seems to have turned out
okay. I put my order in right before Chinese New Year, so it took a bit longer
than I expected though.

Automated PCB assembly definitely beats hand assembling boards by placing hundreds
of components. However, for prototypes, hand assembly still has some advantages
since it can save time and money, so I think I'll still be hand assembling
plenty of boards in the future.

## Controllers for sale

I hope to have some controllers ready to sell next week. There a couple of
things I wanted to go over with the firmware before I'm ready to ship them, and
I'm waiting on the 2.0mm pin headers to go with the controllers.  I'm planning
to sell them for 16USD. If everything goes well, I'd hope to get this price
lower in the future with larger orders.

[Comments on reddit](https://www.reddit.com/r/MechanicalKeyboards/comments/85b41a/update_on_keyplus_usb_typec_and_wireless_split/)
