---
layout: post
title:  "jemini wireless split build log"
date:   2018-04-09
categories: keyboards
---

{% include image.html url="/assests/imgs/build_log1/0.jpg" description="Wireless split with third keyboard as the wireless receiver." %}

{% include image.html url="/assests/imgs/build_log1/1.jpg" description="Time to remove the old controller." %}

This is a rebuild of my old board, you can see the [original build log for with
the case and wiring here](https://imgur.com/a/obCG2). If you are interested
in the files for the case, you can [find them here](https://github.com/ahtn/jemini_layered_acrylic_case). I should probably rework the case to have a hole for the USB
port and better place to position the wireless controller.  When I originally
made this board, I didn't have a way to support USB in the firmware with the
controller I was using in my early prototypes, so that's why there's no USB
port in this version of the case.

{% include image.html url="/assests/imgs/build_log1/2.jpg" description="Wiring up the nRF24L01+ wireless module." %}
{% include image.html url="/assests/imgs/build_log1/2.5.jpg" description="Pinout of the nRF24L01+ module" %}

{% include image.html url="/assests/imgs/build_log1/3.jpg" description="Wiring the battery and the nRF24L01+ module together." %}
{% include image.html url="/assests/imgs/build_log1/4.png" description="Schematic for the wiring" %}
{% include image.html url="/assests/imgs/build_log1/5.jpg" description="Testing wireless communication between modules." %}

At this point I had two wireless modules wired up together, I could start
testing the wireless functionality. To do this I needed to write a basic layout
and assign the device `ID` to each controller. Using the keyplus flasher, you
can generate the RF key and set the device ID using the "Device and RF" tab
from the drop down menu. You only need to do this once, then future updates
will only need to provide your layout file. An example layout file is
given below that defines the two devices and shows how the split devices
are connected. You can
[get latest version of the flasher here](https://github.com/ahtn/keyplus/releases).
You can get more information and examples of the [layout format here](
https://github.com/ahtn/keyplus/tree/master/layouts).

I could then test that the wireless communication was working by shorting the
row and column pins together to generate key presses.

```yaml
# jemini acrylic test

version: 0.3.0

devices:
  "jemini acrylic - left":
    id: 0 # unique id that identifies this device
    wireless_split: true # enables nRF24 wireless support
    wireless_mouse: true # allows pairing Logitech mice
    wired_split: false # not using wired split here
    layout: split_layout # which layout this device uses
    layout_offset: 0 # 1st split device in layout
    scan_mode:
      mode: col_row # matrix with diodes from columns to rows
      rows: 4
      cols: 6
      # maps how keys are wired in the matrix, to how they
      # appear visually. The order the keys are given here,
      # will map to them to the corresponding position in
      # the layout below.
      matrix_map: [
          r3c4, r3c5, r3c2, r3c3, r3c0, r3c1,
          r2c4, r2c5, r2c2, r2c3, r2c0, r2c1,
          r1c4, r1c5, r1c2, r1c3, r1c0, r1c1,
          r0c0, r0c1, r0c2, r0c5,
      ]


  "jemini acrylic - right":
    id: 1
    wireless_split: true
    wireless_mouse: true
    wired_split: false
    layout: split_layout
    layout_offset: 1 # 2nd split device in layout
    scan_mode:
      mode: col_row
      rows: 4
      cols: 6
      matrix_map: [
          r0c1, r0c2, r0c3, r0c0, r0c4, r0c5,
          r3c1, r3c2, r3c3, r3c0, r3c4, r3c5,
          r2c1, r2c2, r2c3, r2c0, r2c4, r2c5,
          r1c4, r1c0, r1c1, r1c2,
      ]

layouts:
  split_layout:
    default_layer: 0
    # Note: 3 layers deep, since you can have multiple
    # split devices in one layer. If you want the devices
    # to maintain layer state separately, you can place
    # them in separate layouts.
    layers: [
      [ # layer 0 (colemak)
        [ # left hand (split device 0)
          0   , q   , w   , e   , r   , t   ,
          1   , a   , s   , d   , f   , g   ,
          2   , z   , x   , c   , v   , b   ,
          3   , 4   , 5   , 6   ,
        ],
        [ # right hand (split device 1)
          y   , u   , i   , o   , p   , 0   ,
          h   , j   , k   , l   , ;   , 1   ,
          n   , m   , "," , "." , "/" , 2   ,
          3   , 4   , 5   , 6   ,
        ],
      ],
    ]
```

{% include image.html url="/assests/imgs/build_log1/6.jpg" description="Next I soldered the controller to the switch matrix." %}
{% include image.html url="/assests/imgs/build_log1/7.jpg" description="Then I could use the second module as a wireless receiver to test first half." %}
{% include image.html url="/assests/imgs/build_log1/8.jpg" description="Repeated for the other half, using the first as a wireless receiver to test the second one. I'm definitely not winning any awards for neat wiring." %}
{% include image.html url="/assests/imgs/build_log1/9.jpg" description="Putting it back together." %}


After reassembling the boards, I notice that some of the keys on the left half
were chattering. This is probably because my soldering on the left half was
pretty janky, so the scanning algorithm didn't wait long enough for signal
to stabilise. To fix that I changed the debounce settings to wait longer. I
could probably have used lower values, but since this case doesn't expose
the USB port externally, I just choose very conservative values to avoid
disassembling the case every time I wanted to change it. I strongly recommend
having an exposed USB port, and it also allows you to use the board as a
wireless receiver for other devices.

The right side seemed to be work fine with the default values, so I just left
it with the defaults values.

```yaml
    scan_mode:
      # ...
      debounce:
        # After an up/down event is generated, the keyboard
        # will wait this long before accepting the next
        # key up/down event (in ms)
        debounce_time_press: 5 # default 5
        debounce_time_release: 5 # default 5

        # switch must stay pressed/relased this long
        # before a key down/up event is generated (in ms)
        trigger_time_press: 5 # default 1
        trigger_time_release: 5 # default 3

        # amount of time between scanning rows (in µs)
        parasitic_discharge_delay_idle: 30.0 # default 2.0
        parasitic_discharge_delay_debouncing: 30.0 # default 10.0
```

{% include image.html url="/assests/imgs/build_log1/0.jpg" description="Connecting it to one of my other split keyboards." %}

After I got everything working, I merged the settings into my main layout file,
you can [see it here](https://github.com/ahtn/keyplus/blob/75c7571233ccc901496a8239a724ca65ee7cd193/layouts/basic_split_test.yaml). This way I can use any of my split
keyboards as a wireless receiver for any of the others. It is important to
remember that the device acting as the wireless receiver determines what
keypress will be generated. Most of the device settings (RF, matrix, etc) are
stored on the device, but the layout used is determined by the receiver. So
if you wanted you could store different layouts on each device, and the layout
used would be determined by which device is acting as the receiver.

[Comments on reddit](https://www.reddit.com/r/MechanicalKeyboards/comments/8ays8u/wireless_split_build_log/)
