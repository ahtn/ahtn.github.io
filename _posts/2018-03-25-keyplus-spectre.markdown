---
layout: post
title:  "keyplus spectre"
date:   2018-03-25
categories: keyboards
---

{% include image.html url="/assests/imgs/first-prototypes/keyplus-spectre.jpg" description="I'm really happy with how the routing came out." %}

Since I started using xmegas for my keyboard projects, I wanted to take
advantage of some of the different variants. In this build I used 64 pin xmega
to wire each switch to a GPIO pin on the controller. The xmega doesn't need
many support components for USB, so the schematic is very mimimal:

{% include image.html url="/assests/imgs/first-prototypes/spectre-schematic.png" %}

Features:

* MCU: [atxmega32c3](http://www.microchip.com/wwwproducts/en/ATxmega32C3) (64 pin, 50 GPIO) (very cheap [on digikey](https://www.digikey.com/product-detail/en/microchip-technology/ATXMEGA32C3-MHR/1611-ATXMEGA32C3-MHRCT-ND/6833405) atm)
* USB 2.0 Type-C
* MX, ALPS, and Kailh low profile switch footprints

You can find the [KiCAD schematics files here](https://github.com/ahtn/keyboard_pcb/tree/master/spectre), and the keyplus [layout file
is here](https://github.com/ahtn/keyplus/blob/9d320d65f64290681a73cb76dadb0254b90d6d53/layouts/spectre.yaml). 
I still need design a case though. I'll probably 3D print one.


## keyplus controller update

Also, for those interested about the keyplus USB Type-C controller boards I posted
about last week, I have another [batch of boards to sell here](https://www.reddit.com/r/mechmarket/comments/86zhpa/au_h_keyplus_usb_typec_keyboard_controllers_w/).


[Comments on reddit](https://www.reddit.com/r/MechanicalKeyboards/comments/86zo7g/keyplus_spectre_pcb_4x12_ortho_without_diodes_usb/)
