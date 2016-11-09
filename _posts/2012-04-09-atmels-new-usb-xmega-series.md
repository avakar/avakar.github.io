---
title: Atmel's new USB XMEGA series
tags: [robotics, atxmega]
---

A package with Atmel's [atxmega32a4u][1] finally arrived on Thursday.
Although I've been waiting for the chip to become available for months,
I've only gotten to etch [a demo board][2] for it on Friday night
(what's one to do on Friday night after all).

  [1]: http://www.atmel.com/devices/atxmega32a4u.aspx
  [2]: https://technika.junior.cz/trac/browser/demo_boards

~

Anyway, I have a working board now, which I can easily program with
my [Shupito programmer][3] (shameless plug). I also got to try out
a [micro USB recepticle][4].

<img style="display: block; margin-left: auto; margin-right: auto" src="/images/IMG_20120409_111541.jpg" alt="atxmega32a4u demo board" width="320" height="240" />

A few interesting things popped out.

 * From the programming perspective, the chip identifies itself with
 the same signature as it's cousin chip, atxmega32a4; in fact,
 all atxmega32a chips identify themselves the same.
 * The micro USB connector was difficult to align. Then again, I was
 probably doing it wrong -- it's an SMD component and I used tweezers
 to hold it in place in the same way I would an SMD resistor. What I should
 have done instead is to lay the connector on the board and use
 the tweezers to carefully push it to alignment (like you would a TQFP).
 * I shouldn't design boards in rush, I've completely missed
 on the difference between the 5V on the USB bus and the 3.3V
 maximum supply voltage of XMEGA. What a newbie mistake.
 Fortunately, I realized it the next morning (well, noon,
 you know, right after I woke up) before I plugged the cable in.
 Cutting a trace on the board solved the problem; still, I have
 to provide the supply voltage externally. There really should have been
 an LDO to allows the board to be powerd from USB.
 * Trying to rotate a crimp socket on a standard (square) pinhead
 to get a better contact is counter-productive. It turns out they lose
 springiness very easily -- to a point where you can pretty much throw
 them away.
 * Putting a transparent coating on a board will prevent it from corroding.
 Putting it on a pinhead will prevent electrical contact. Perhaps you can
 mechanically scrape the coating off by rotating the socket?
 * Atmel provides [a USB bootloader for XMEGA][5] with [sources and
 precompiled binaries][6]. I don't think it was preprogrammed in the chip
 though (I might have accidentally erased it, I'm not sure). You need
 to download [Atmel's FLIP][7] software to get it running, or you can
 [write your own][8].

I'm thinking of implementing the FLIP protocol into
my [avr232client][9] or to [Lorris][10], although neither is particularly
suited for raw USB connections.

I'd also like to try out the [Atmel's software framework][11], especially
for the USB functionality (the download requires [registration][12]).

Last but not least, I should finally port the rest of avr232client to Lorris,
but there are lots of details that need to be taken care of and I'm not sure
how much time it would take (also, it's GPL and Qt, neighter of which
particularly motivates me).

  [3]: http://shupito.net
  [4]: http://cz.farnell.com/jsp/search/productdetail.jsp?SKU=1568026
  [5]: http://www.atmel.com/Images/doc8429.pdf
  [6]: http://www.atmel.com/Images/AVR1916.zip
  [7]: http://www.atmel.com/tools/FLIP.aspx
  [8]: www.atmel.com/Images/doc8457.pdf
  [9]: https://technika.junior.cz/trac/wiki/avr232client
  [10]: http://tasssadar.github.com/Lorris/index.html
  [11]: http://www.atmel.com/tools/AVRSOFTWAREFRAMEWORK.aspx
  [12]: http://www.atmel.com/Images/asf-standalone-archive-3.0.1.zip
