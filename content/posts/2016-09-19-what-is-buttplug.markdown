Title: What is Buttplug?
Date: 2016-09-19 18:30
Category: Buttplug Development
Tags: buttplug, software
Slug: what-is-buttplug

<center><img src='/images/2016-09-19-what-is-buttplug/buttplug.svg' width=50% height=50%></center>

I've been digitally pontificating about sex and technology since
2004. During that time, I've noticed a recurring call for a unified
protocol for controlling sex tech hardware. This would give developer
a generic stream of data that could define certain properties of a
playback or communication situation. 

For instance, imagine a movie that has abstract data embedded in it
about pressure, speed, friction, and other metrics needed to replicate
the physical situation being portrayed. Toys that translate this data
already exist, but movies usually have to be encoded for a specific
toy. That toy may not work for all users, due to physical differences,
likes, dislikes, etc. What if we could make the data abstract enough
to work with any toy? Could we take that beyond movies too, so that
other interactive experiences such as games could harness whatever
hardware the user enjoyed most?

Every time this idea was mentioned, I've argued against it, due to the
amount of permutations it would have to encompass. Some toys vibrate,
some use friction, some use direction electrical stimulation of
nerves, some are one-offs beyond my knowledge and/or imagination. How
could we possibly make a decent mapping of all of the data in one
protocol without bloating into ridiculousness?

I never asked was "is it worth it to even try?". Every time the
subject came up, I just blew it off. 

Lately, I've realized that the core system of this idea would actually
be fairly simple. A simple routing core that client applications could
converse with, allowing them to control toys, and receive data from
sensors in those toys. That's it. This is a workflow that comes up
constantly in other fields (examples
include
[VRPN](https://github.com/vrpn/vrpn),
[OSCulator](http://www.osculator.net/),
[FreePIE](http://andersmalmgren.github.io/FreePIE/), etc), so why not
try doing it for sex toys?

Thus, [Buttplug](https://github.com/metafetish/buttplug/).

[Buttplug](https://github.com/metafetish/buttplug/) is an application
that will allow developers to quickly and easily control or read data
from as many different sex hardware products as we can support. It
will have IPC built in over both local TCP and websockets, so that
ideas for control and interactiveity can be prototyped as either
native applications or web apps. It should be embeddable, so that it
can be linked into larger interactive applications like games and VR
experiences, allowing developers to integrate intimate haptics via a
safe, simple process. All of these features, as well as design
considerations and development processes, will be outlined here in
upcoming blog posts.

Buttplug may be my magnum opus, the culmination of over a decade of
sex tech research. It may also be a dismal failure that ends up a
bitrotted artifact floating around the net. No matter the outcome,
it's worth a shot.

