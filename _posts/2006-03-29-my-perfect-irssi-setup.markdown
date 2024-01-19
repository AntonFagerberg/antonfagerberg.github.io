---
  layout: post
  title: "My perfect Irssi setup"
  alias: [/archive/my-perfect-irssi-setup, /texts/my-perfect-irssi-setup/]
  categories: blog
  tags: misc
---

## Overview

 * Installation
 * Basic Irssi usage
 * Scripts
 * Themes
 * BitlBee
 * Setting up BitlBee with Twitter and Google Talk
 * Nicklist
 * Xchat nick color
 * Hilight window
 * Advanced Window List
 * Auto Away
 * Autoreply (BitlBee)
 * Status notice (BitlBee)
 * OTR encryption for Irssi and BitlBee
 * Notifications
 * Last.FM

## Installation

This guide focuses on Ubuntu since it is one of the most common Linux distributions. Everything else in the guide is however the same everywhere.

To install everything we need type:

```bash
sudo apt-get install irssi bitlbee screen bitlbee-plugin-otr libtime-duration-perl libnotify-bin irssi-plugin-otr
```

This will install the latest version of Irssi, BitlBee 3 and Screen. It is important that you have Backports or you will not get Bitlbee 3 (this statement is probably deprecated). You need screen for the nicklist-script and it is totally awesome on its own so you should learn to use it anyway.

Learn more about Irssi and Screen: [quadpoint.org/articles/irssi](http://quadpoint.org/articles/irssi)

Now type irssi in your terminal to get going!

### Basic Irssi usage

All Irssi-commands starts with a forward slash `/`. You can use the tab-key to auto-complete commands and you can browse through all commands by pressing tab several times.

First of all you should set your nickname:

```text
/nick my-cool-nickname
```

If you want to connect to a IRC-server you can use either `/server` or `/connect`. Using `/server` will kill the old connection (if you have one) but using `/connect` will establish a new parallel connection so that you can have several active connection.

If you want to auto connect and join channels on start up you can use:

```text
/server ADD -auto -network NetworkName irc.host.com 6667
/channel ADD -auto #channel NetworkName password
```

To switch between servers press `CTRL + X`. To switch between windows / channels press `ALT + <number>` or use:

```text
/window number
```

To use the ALT-combination on numbers higher than 10 (which uses key 0) you can use the letters below the numbers on a qwerty-keyboard. `ALT+Q = 11`, `ALT+W = 12` and so forth.

To close a window use:

```text
/wc
```

When you have customized Irssi as you wanted it, execute:

```text
/save
```

This will save all your settings and apply the current theme.

[Consult the official manual to get more information about commands and features.](http://irssi.org/documentation/manual"> http://irssi.org/documentation/manual)

### Scripts

The really great thing about Irssi is that there are thousands of scripts and customizations. I will walk you through my favorite scripts further down.

Irssi scripts are written i Perl and are usually located in `~/.irssi/scripts/`. To load a script you can use the /script command:

```text
/script load mycoolscript.pl
```

Or you can auto-load the scripts so you don't have to use the script command every time you start Irssi. You can do this by putting the .pl-file in a folder called autorun in the scripts folder `~/.irssi/scripts/autorun/`.

You can find a lot of scripts on Irssi's official script site:
[scripts.irssi.org](http://scripts.irssi.org/).

### Themes

There are tons of themes. I use a theme called xchat which you will see in this guide. You can find a lot of themes if you Google for it or visit the official Irssi Theme page: [irssi.org/themes](http://irssi.org/themes).

The xchat theme is located at [irssi.org/themefiles/xchat.theme](http://irssi.org/themefiles/xchat.theme).

To load a theme (located in `~/.irssi/`), use:

```text
/set theme xchat
```

Replace xchat with the name of your theme of choice.

![irssi overview](/images/tutorials/irssi3.png)

Note that I use a special script with this theme. You can read about it further down.

## Bitlbee

BitlBee is an IRC to other networks gateway. This means that you can use other protocols like XMPP/Jabber (used by Google Talk and Facebook chat), MSN Messenger, Yahoo! Messenger, AIM and ICQ from inside an IRC-client like Irssi. You can connect to a public server but there is no hassle setting up your own which means that your information (like passwords and chat records) isn't transmitted to any unknown servers.

This guide uses BitlBee 3.0.1 which you can install in Ubuntu Maverick or greater by executing:

```text
sudo apt-get install bitlbee
```

You can control the BitlBee service from your terminal:

```text
sudo service bitlbee {start|stop|restart|force-reload}
```

The config file is located at `/etc/bitlbee/bitlbee.conf` . Look it up and customize it for your own needs. You need to restart the service if you make any changes to the configuration file.

When you are ready to connect to your BitlBee server (and if you haven't changed any configurations) execute the following in Irssi:

```text
/connect localhost
```

Once you are connected you should get a channel called BitlBee. This is where your contacts will be located and where you execute all your BitlBee commands. The first thing you should do is to go to the channel and register a new account.

Do this by typing this in the BitlBee channel:

```text
register (password)
```

Make sure that you have the nickname which you want to use and note that we want to send command to the Bitlbee-channel and not to Irssi so make sure you don't have a / in front of any commands to Bitlbee.

The next time you return to Bitlbee you'll use:

```text
identify (password)
```

To load all of your accounts.

What we need to do now is to add one of your IM-accounts to Bitlbee. The syntax for this is:

```text
account add (protocol) (username)
/OPER
```

Note that we used the Irssi command /OPER to hide the password instead of typing it directly to BitlBee.

To list all of your accounts:

```text
account list
```

All accounts are numbered and these numbers are used when modifying and connecting etc.

To bring an account online use.

```text
account (number) on
```

If you do not provide any account number all of your accounts will be connected.

There is a help command which you can use if you want to know about the syntax of any of the commands. Usually you'll do it in several steps:

```text
help account
help account add
help account add msn
```

You might want want to stript out html-tags from the incoming messages since our client is text based. Do this with the following Bitlbee-command:

```text
set strip_html true
```

I will guide you how to use Twitter and Google Talk in Bitlbee. You should have no problem adding other accounts the same way.

### Google Talk

```text
account add jabber (email)
/OPER
account (number) set server talk.google.com:5223:ssl
```

The account is now ready to be brought online!

### Twitter
This setup is a bit different since Twitter use OAuth.

Firstly do as normal:

```text
account add twitter (username)
/OPER
```

The new twitter user will send you a PM. With a link for the OAuth authentication. Open the link in your browser, sign in and press Allow. Get the numeric code and send it back to the twitter user in the same window.

You should now be signed in to your twitter account. If you like you can rename this or any other user:

```text
rename twitter_username twitter
```

Once signed in a new channel will be open which displays all tweets from your contacts.  Send a message in this channel to post a status update to Twitter.

## Nicklist
This script places all nicknames in a channel in a bar at the side of the window like many other IRC-channels. You can use it in two ways. None of them is perfect but I prefer using it together with Screen.

You have to start Irssi inside a Gnu Screen session for it to work. You can do this directly from the console by typing:

```text
screen irssi
```

You must specify the script to use Screen from inside Irssi:

```text
/nicklist screen
```

Download: [scripts.irssi.org/scripts/nicklist.pl](http://scripts.irssi.org/scripts/nicklist.pl)

Learn more about Irssi and Screen: [quadpoint.org/articles/irssi](http://quadpoint.org/articles/irssi)

![nicklist](/images/tutorials/nicklist2.png)

## Xchat nick color

This script works together with the xchat-theme (as you can find if you scroll up a bit) and does two very important things. First of all, it aligns all nicknames to the right in the chat area which creates a vertical line with the chat messages. This makes it a lot easier to read. And secondly, it will give all nicknames a unique color so it is easier to see who sends what.

Download: [dave.waxman.org/irssi/xchatnickcolor.pl](http://dave.waxman.org/irssi/xchatnickcolor.pl)

![nick color](/images/tutorials/irssi_nickcolor1.png)


## Hilight window

Every time you get hilighted (someone types your nickname or any other higlighted word or sends you a private message) a copy of that message will be sent to a separate window.

A great option is to display that window on the top of the terminal at all time. That way you'll never miss a message.

```text
/window new split
/window name hilight
/window size 4
```


Download: [scripts.irssi.org/scripts/hilightwin.pl](http://scripts.irssi.org/scripts/hilightwin.pl)

More info: [quadpoint.org/articles/irssi#hilight\_window](http://quadpoint.org/articles/irssi#hilight_window)

![hilight](/images/tutorials/hilight1.png)

## Advanced Window List

AWL customizes the list at the bottom which displays the window list with channels etc.

This is my configuration which I've stoled from the link below:

```text
/set awl_display_key $Q%K|%n$H$C$S
```

Download: [anti.teamidiot.de/static/nei/\*/Code/Irssi/adv\_windowlist.pl](http://anti.teamidiot.de/static/nei/*/Code/Irssi/adv_windowlist.pl)

More info: [quadpoint.org/articles/irssi#channel\_statusbar\_using\_advanced\_windowlist](http://quadpoint.org/articles/irssi#channel_statusbar_using_advanced_windowlist)


![awl](/images/tutorials/awl1.png)

## Auto away

Another script to use together with Screen. It marks you as away when Screen detects that you are not active in the session.

```text
/set screen_away_active ON
/set screen_away_message
/set screen_away_nick
```


Download: [scripts.irssi.org/scripts/screen\_away.pl](http://scripts.irssi.org/scripts/screen_away.pl)

## Auto-reply Bitlbee
This will reply to all incoming conversation within BitlBee with a custom message when you are away. This of it as an answering machine.

You will need the Perl library time-duration. Ubuntu users can use:

```text
sudo apt-get install libtime-duration-perl
```


And to activate it in Irssi:

```text
/set bitlbee_autoreply_duration ON
```


Download: [github.com/msparks/irssiscripts/raw/master/bitlbee\_autoreply.pl](https://github.com/msparks/irssiscripts/raw/master/bitlbee_autoreply.pl)

![auto reply](/images/tutorials/irssi_autoreply1.png)

## Status notice (BitlBee)

Displays additional information when a users change status such as how long the user was marked as away / offline.

Download: [github.com/msparks/irssiscripts/raw/master/bitlbee\_status\_notice.pl](http://github.com/msparks/irssiscripts/raw/master/bitlbee_status_notice.pl)

![status notice](/images/tutorials/irssi_status_notice1.png)


## OTR encryption for Irssi & BitlBee

Installation for Ubuntu users:

```text
sudo apt-get install bitlbee-plugin-otr irssi-plugin-otr
```


You need to load the OTR module inside Irssi:

```text
/LOAD otr
```


If you wish to do it automatically add this to or create the file ~/.irssi/startup:

```text
LOAD otr
```

Note that we do not use forward-slash in this file.

Irssi OTR settings:

```text
Commands:

/otr genkey nick@irc.server.com
  Manually generate a key for the given account(also done on demand)
/otr auth [<nick>@<server>] <secret>
  Initiate or respond to an authentication challenge
/otr authabort [<nick>@<server>]
  Abort any ongoing authentication
/otr trust [<nick>@<server>]
  Trust the fingerprint of the user in the current window.
  You should only do this after comparing fingerprints over a secure line
/otr debug
  Switch debug mode on/off
/otr contexts
  List all OTR contexts along with their fingerprints and status
/otr finish [<nick>@<server>]
  Finish an OTR conversation
/otr version
  Display irc-otr version. Might be a git commit

Settings:

otr_policy
  Comma-separated list of "<nick>@<server> <policy>" pairs. See comments
  above.
otr_policy_known
  Same syntax as otr_policy. Only applied where a fingerprint is
  available.
otr_ignore
  Conversations with nicks that match this regular expression completely
  bypass libotr. It is very unlikely that you need to touch this setting,
  just use the OTR policy never to prevent OTR sessions with some nicks.
otr_finishonunload
  If true running OTR sessions are finished on /unload and /quit.
otr_createqueries
  If true queries are automatically created for OTR log messages.
```


OTR for BitlBee is loaded automatically when installing the Ubuntu package. Use the help command in BitlBee to get information about how to use it:

```text
help otr
```


## Notifications
This script provides notifications when you get hilighted via libnotify. In Ubuntu you need to install libnotify-bin:

```text
sudo apt-get install libnotify-bin
```

It works with SSH and GNU Screen but remember to forward X when using SSH with the -X switch.


Download: [irssi-libnotify.googlecode.com/svn/trunk/notify.pl](http://irssi-libnotify.googlecode.com/svn/trunk/notify.pl)

More info: [code.google.com/p/irssi-libnotify](http://code.google.com/p/irssi-libnotify/)


![notification](/images/tutorials/notification1.png)

## Last.FM

I use this to show my friends what I&#8217;m listening to. It prints out the last played song from Last.FM.

Set your username:

```text
/set lastfm_user
```


And show what's playing:

```text
/np
```


Download: [scripts.irssi.org/scripts/lastfm.pl](http://scripts.irssi.org/scripts/lastfm.pl)


## Official sites

[irssi.org](http://irssi.org/)

[bitlbee.org](http://bitlbee.org/)

[irssi-otr.tuxfamily.org](http://irssi-otr.tuxfamily.org/)


## More help

Official Irssi documentation:
[irssi.org/documentation](http://irssi.org/documentation)

Official BitlBee wiki:
[wiki.bitlbee.org](http://wiki.bitlbee.org)


Quadpoint - the best known Irssi guides:
[quadpoint.org/articles/irssi](http://quadpoint.org/articles/irssi)
