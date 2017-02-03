title: About Buttplug
slug: about

# Buttplug - For Users

Let's say you just bought the shiny new computer controlled sex toy of
your dreams. Sure, it comes with its own control software, or maybe it
has some content (translation: porn) it synchronizes to, but you've
found other stuff that works better for your needs. 

Up until now, you'd be stuck trying to figure out how to accommodate
your needs with whatever the toy manufacturer gives you. Buttplug is a
way around that. Buttplug software allows developers to easily build
applications, games, and other software that will control all sorts of
toys, instead of just one type of toy.

In terms of extensibility, Buttplug is built to take care of both
simple and exotic needs. What if the application you want to use only
supports vibration, but you want to use it with electrostim? It may
not be a perfect replication of the feeling, but Buttplug provides a
way to at least try that kind of pairing. Be it a normal bluetooth
vibrator, all the way up to position controlled fucking machines,
Buttplug is there to make sure it works.

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

- [VRPN](https://github.com/vrpn/vrpn),
- [OSCulator](http://www.osculator.net/),
- [FreePIE](http://andersmalmgren.github.io/FreePIE/) 

All of these allow users to take an arbitrary set of supported
controls, and use them in ways that are customized to their needs.
Buttplug has a similar aim, except instead of Wiimotes and VR Gloves,
it focuses on computer controlled sex toys.

## Limitations

tl;dr - Buttplug is fast but not THAT fast. Don't plan on sending
audio sample-by-sample via Buttplug.

Due to the nature of the hardware it will be controlling, it is
assumed that most data going through buttplug will be fairly low
bandwidth. For instance, a buttplug server might be used with a estim
machine to trigger a pattern to be played. However, running the
pattern step by step is not something that a buttplug server, nor the
protocol, would be designed to do. The focus for the protocol and
servers is on high level control information. While parametric
commands can be sent, it is assumed that something other than a
buttplug server would be generating signals based on those commands.

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

The buttplug protocol is a JSON based protocol. Buttplug protocol
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

## Possible Buttplug Server Features

### Hardware Discovery

### Connections and Communication

### Command Filtering and Adaptation
