title: About Buttplug
slug: about
template: hardware
og_image: images/buttplug.png

# Buttplug - For Users

Let's say you just bought the shiny new computer controlled sex toy of
your dreams. Sure, it comes with its own control software, or maybe it
has some content (translation: porn) it synchronizes to, but you've
found other stuff that works better for your needs. 

Up until now, you'd be stuck trying to figure out how to accommodate
your needs with whatever the toy manufacturer gives you. Buttplug is a
way around that. Buttplug software allows developers to easily build
applications, games, and other software that will control all sorts of
toys, instead of just one type of toy. Better yet, Buttplug is open
source, so if you want to write your own software to do whatever it is
you want, you have access to all the functionality you need.

Buttplug is built to take care of both simple and exotic sex toy
needs. What if the application you want to use only supports
vibration, but you want to use it with electrostim? It may not be a
perfect replication of the feeling, but Buttplug provides a way to try
that kind of pairing. Be it a normal bluetooth vibrator, all the way
up to position controlled fucking machines, Buttplug is there to help
make sure you get off in the way you want.

# Buttplug - For Developers

Buttplug is a set of specifications and applications that allows users
of sex toys to be able to use the toy they like with the applications
they want. The API gives developers a way to write compact, simple
code that will control multiple sex toys, possibly including ones not
even on the market yet!

While Buttplug will be one of the first applications to control
multiple sex toys, software that centralizes and scripts controls to
suit the needs of users has been around for years. For instance,
programs and libraries such as:

- [VRPN](https://github.com/vrpn/vrpn)
- [OSCulator](http://www.osculator.net/)
- [FreePIE](http://andersmalmgren.github.io/FreePIE/) 

All of these allow users to take an arbitrary set of supported
controls, and use them in ways that are customized to their needs.
Buttplug has a similar aim, except instead of Wiimotes and VR Gloves,
it focuses on computer controlled sex toys.

## Limitations

tl;dr - Buttplug is fast but not THAT fast. Don't plan on sending
audio sample-by-sample via Buttplug.

Due to the nature of the hardware it will be controlling, it is
assumed that most data going through Buttplug will be fairly low
bandwidth. For instance, a Buttplug server might be used with a estim
machine to trigger a pattern to be played. However, running the
pattern step by step is not something that a Buttplug server, nor the
protocol, would be designed to do. The focus for the protocol and
servers is on high level control information. While parametric
commands can be sent, it is assumed that something other than a
Buttplug server would be generating signals based on those commands.

## Architecture

Buttplug consists of two components:

- A protocol, which is simply a specification for talking to specific
  and generalized groups of sex toys.
- A server, which is a concrete implementation of the protocol that
  may also provide utility services like device discovery and
  management, connection handling, and command filtering.

The protocol serves as a template for implementing Buttplug servers.
Buttplug servers can then be implemented in any language/platform
required, with some basic assumption that they will all share at least
some similar traits.

## Buttplug Protocol

The Buttplug protocol is a JSON based protocol. Buttplug protocol
packets consist of:

- A packet type
- A packet name
- Command data

The protocol is defined in layers. At the lowest layer, each supported
toy will have a "Raw" message type. Raw messages allow developers to
send commands specific to the packet type for the toy. For example,
toys that use the Lovense protocol would have a "RawLovense" message,
the Command Data portion of which would be a valid Lovense command.

On top of Raw messages will be a set of "Generic" messages. For
instance, there might be a message named "VibrationSpeed" which takes
a speed value of 0 (full off) to 100 (full on). Instead of having to
worry about knowing which toy a user might have and sending the "Raw"
message for that toy type, developers can send "VibrationSpeed" and it
will be translated for whatever toy the user might have, assuming the
server and toy supports that message.

## Buttplug Servers

Servers are specific implementations of the Buttplug protocol.
Usually, they'll implement some way to talk to hardware, and some way
for software to connect to the server. How they implement this depends
on the platform and hardware they're trying to support.

A few examples of possible servers:

- a desktop server with capabilities for discovering devices across
  multiple transports (usb, serial, bluetooth, etc), allowing client
  to talk to the hardware via network, websockets, or local sockets.
  It could also expose different levels of information to clients
  depending on privacy desired by users (more on this in the
  application examples section).
- an embeddable library so that video games could connect to whatever
  toys are usable in that version of the Buttplug server library.
- a javascript based server that could be embedded in webapps, that
  would allow Bluetooth LE toys to connect to the app via
  WebBluetooth.

## Buttplug Application Examples

A few examples of application ideas are listed here to show how
Buttplug can work in various situations.

### Desktop Server with Web Client

In this case, a desktop server is run by the user. This server can
expose two levels of privacy about the hardware the user has:

- Full Exposure: The hardware make and model are exposed to clients.
  This would allow the client to send device specific messages to the
  hardware, but would also possibly compromise some privacy for the
  user. For instance, if the user was using a Lovense Lush/Hush, both
  of which have one vibration motor, the client could send either the
  "VibrationSpeed" Generic message or the "RawLovense" Raw message, to
  control the toys.
- Limited Exposure: Only generic information is send about the
  hardware. Revisiting the previous example, if the user has a Lovense
  Lush/Hush toy, the only information that would be exposed is that is
  supports the "VibrationSpeed" Generic message. "RawLovense"
  capabilties would not be provided to the client.

The Desktop server would also have the ability access all transports
available in the operating system it was on, meaning that current
Bluetooth toys could be used, as well as legacy USB, Serial, and other
toys.

Once the user brings up the Web Client, it connects to the Desktop
Server via WebSockets (similarly, a Desktop Client could connect via
regular sockets, or local sockets if that was available). The Web
Client can then query the hardware available, and set up interaction
accordingly.

To get more specific, let's look at an actual Buttplug application.
Giftic is a web application that takes an animated gif, analyzes the
motion in the gif, and relays that as a set of commands to a toy. The
application makes some assumptions about the gif, like that it will
most likely contain looped reciprocal movement. Giftic is capable of
sending two types of information: a magnitude, and 1d direction of
movement. This could be translated into two different types of
Buttplug messages. For toys with only vibration, the magnitude could
be translated into a VibrationSpeed message, so that the larger the
movement between two frames, the faster the toy would vibrate. Some
toys, like the RealTouch or a fucking machine, also have linear
movement. A VectorMovement message which denotes both magnitude and
either 1 or -1 for direction could be sent to these toys, to mirror
the movement in the gif more accurately.

### Web Server/Client

What if the user doesn't want to run a desktop application, though?
Let's continue the giftic example, except this time instead of talking
to a server running on the desktop, Giftic uses a javascript server.

Due to how Web APIs work, the user will lose a certain amount of
privacy guarentees. Using either WebBluetooth or the Gamepad API will
expose information about the hardware to the browser, though with
WebBluetooth, this is only through the user's request. The "Limited
Exposure" option of the desktop server isn't viable here though.

The tradeoff is that with a Buttplug server running inside the
webpage, nothing needs to be installed on the user's computer. The
browser comes with all the necessary functionality to access some
toys, which may be enough for most users. Additionally, the browser
may support platforms that would otherwise be difficult to access,
like mobile phones.

The only different the giftic application sees is that instead of the
Buttplug commands going over a websocket, it now goes to an internal
Buttplug server that handles the bluetooth calls.

### Embedded Server in a Game

Going back to desktop, let's say there's a video game that would like
to access and use sex toys via Buttplug. There's no reason to require
the user to run an outside server at this point, since they're already
running the video game application. A Buttplug Server could simply be
linked in as a library, Unity Component, etc... with all of the
functionality expected from a desktop server, and things like device
I/O threading handled for the game without the developer having to
worry about it. Communication could be set up via a set of
non-blocking functions and a buffer to relay messages.
