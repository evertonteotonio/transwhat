# transWhat

transWhat is a WhatsApp XMPP Gateway based on [Spectrum 2](http://www.spectrum.im) and [Yowsup 2](https://github.com/tgalal/yowsup).

## Features

  * Typing notifications
  * Receive images, audio & video
  * Set/get online status
  * Set status message
  * Groupchats

## Usage

### Bot

You might want to talk to [[.:bot|our bot]] if you're feeling lonely ;-P

### Login

|**User**|CountryCode + PhoneNumber (eg. 4917634911387)|
|**Password**|WhatsApp password, see [[.:password|password]] for details|
|**Domain**|*example.org*|
|**Port**|5222|
|**Alias**|//not required//|
|**Ressource**|//not required//|
|**Encryption**|//activate!!!//|

### Buddies

WhatsApp does not store your contacts on their servers. Thus you need to import your contacts manually with your XMPP Client or use [[.:bot|our bot]] to Import your contacts from Google (preferred).

(In Pidgin: Menu => Buddys => Add Buddy)

Just use the same JID format as for your login:

  CountryCode + PhoneNumber + "@whatsapp.example.org"

### Groups

To chat with groups you need to add them manually to your XMPP client.

To get a list of your WhatsApp groups, you can use the AutoDiscovery function of your WhatsApp client.

(In Pidgin: Menu => Buddys => Join Chat => RoomList)

When asked, use this conference server:

  whatsapp.0l.de
  
### Smileys / Emojis

To be able to see smileys, you will need an [[https://github.com/stv0g/unicode-emoji/raw/master/symbola/Symbola.ttf|Unicode emoji font]].

When using pidgin, you might want to check out my [[https://github.com/stv0g/unicode-emoji|Unicode emoji theme]].

## Setup

I assume that you have a basic understanding of XMPP and the the concept of a XMPP component / transport. If not, please get a book about Jabber or read the standards.

transWhat is a XMPP transport. By this means it extends the functionallity of an existing XMPP server. It acts as a gateway between the XMPP and WhatsApp networks. It receives WhatsApp messages and forwards them to your XMPP client (and vice-versa).

The implementation of transWhat is based on the [Spectrum 2](http://www.spectrum.im) framework and the [Yowsup 2](https://github.com/tgalal/yowsup) library to interface with WhatsApp.

The following chart summarizes the involved components and the protocols they use to communicate.

### Prosody

##### Installation

I will not cover the installation of Prosody in this guide. Please look for some other tutorials on how to do that.

##### Configuration

The only important thing for us is the configuration of a XMPP component (Spectrum 2 in our case).
See http://prosody.im/doc/components.

Append the following at the end of `/etc/prosody/prosody.cfg.lua`

    Component "whatsapp.0l.de"
        component_secret = "whatsappsucks"
        component_ports = { 5221 }
        component_interface = "127.0.0.1"

### Spectrum 2

##### Installation

Manual compile latest version from [Github](https://github.com/hanzz/libtransport).
You can use the following guide: http://spectrum.im/documentation/installation/from_source_code.html.

##### Configuration

Create a new file `/etc/spectrum2/transports/whatsapp.cfg` with the following content:

    [service]
    user = spectrum
    group = spectrum

    jid = whatsapp.0l.de

    server = localhost
    password = whatsappsucks
    port = 5221

    backend_host = localhost
    backend = /location/to/transwhat/transwhat.py
    
    users_per_backend = 10
    more_resources = 1
    
    admin_jid = your@jid.example
    
    [identity]
    name = transWhat
    type = xmpp
    category = gateway
    
    [logging]
    config = /etc/spectrum2/logging.cfg
    backend_config = /etc/spectrum2/backend-logging.cfg

### transWhat

##### Installation

Checkout the latest version of transWhat from GitHub:

    $ git clone git@github.com:stv0g/transwhat.git
    
Install required dependencies:

    $ pip install --pre e4u protobuf python-dateutil

  - **e4u**: is a simple emoji4unicode python bindings
  - [**yowsup**](https://github.com/tgalal/yowsup): is a python library that enables you build application which use WhatsApp service.

##### Configuration

Then create a new file called `constants.py` in the newly checked out transWhat Git repository:

    BASE_PATH = "/location/to/transwhat"
    
    TOKEN_FILE = BASE_PATH + "/logs/tokens"
    MOTD_FILE = BASE_PATH + "/conf/motd"
    REQUESTS_FILE = BASE_PATH + "/logs/requests"

## Contributors

Pull requests, bug reports etc. are welcome. Help us to provide a open implementation of the WhatsApp protocol.

The following persons have contributed major parts of this code:

  - @stv0g (Steffen Vogel): Idea and initial implementation based on Yowsup 1
  - @moyamo (Mohammed Yaseen Mowzer): Port to Yowsup 2
  - @DaZZZl: Improvements to group chats, media & message receipts

## Links

An *outdated* project wiki is available [here](https://dev.0l.de/wiki/projects/transwhat/).

An *outdated* writeup of this project is also availabe at my [blog](http://www.steffenvogel.de/2013/06/29/transwhat/).
