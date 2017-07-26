# Denon-DRA-F109-Remote
Denon DRA-F109 command line remote control for Linux / Raspberry Pi via Serial Port

For technical setup please take a look at ["Hacking Denon DRA-F109 remote connector" by Kamil Figiela](https://kfigiela.github.io/2014/06/15/denon-remote-connector/)

This CLI script requires Python and [PySerial](https://pythonhosted.org/pyserial/) which should be available in standard repositories.

Instead of a Raspberry PI I use an Intel NUC N2820 in combination with an USB -> TTL RS232 5V PL2303 HX Adapter. Sending commands works well. Nevertheless, so far I was not able to read information from the Denon receiver. I expect that this is because of the low quality of the TTL RS232 adapter. The one that I've bought at Ebay's is unfortunately a fake adapter (see also [here](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=155&pcid=41)) but it works under Linux (and also under Windows 10 by using an out-dated driver)   

I have taken all information about the Denon protocol from
["Complete guide to Denon DRA-F109 control protocol" by Kamil Figiela](https://kfigiela.github.io/2015/10/12/complete-guide-to-denon-dra-f109-control-protocol/)

## Usage
```
$ ./denon.py 
Denon DRA-F109 command line remote control for Linux / Raspberry Pi via Serial Port, e.g. RS-232 / PL2303

USAGE:	denon.py <command1> <params1> <command2> <command2> ...

EXAMPLE:	Set FM radio as input source, select preset 24 and set volume to 12
		$ ./denon.py fm num +10 num +10 num 4 vol 12

  alarm <off|on|once|everyday>	Activates / deactivates alarm clocks
  analog <1|2>            	Sets input source to Analog n, where n is 1 or 2
  balance <left|right>    	Sets balance one step more to left or right
  bass <+|->              	Increases / decreases bass level
  cd                      	Sets input source to CD (digital-in for CD)
  cda                     	Sets input source to CD and selects CD
  clear                   	Sends clear command
  dab                     	Sets input source to DAB radio
  dimmer <high|normal|low|off>	Sets brightness of display
  down                    	Moves in current menu down
  enter                   	Commit current setting by pressing enter
  fm                      	Sets input source to FM radio
  forward                 	Forwards in current title
  info                    	Toggles display
  internet                	Sets input source to Network and selects internet radio
  ipod                    	Sets input source to Network and selects iPod
  left                    	Moves in current menu to the left
  mode                    	Toggles stereo/mono mode
  mute <on|off>           	Mute on/off
  net                     	Sets input source to Network (digital-in NETWORK)
  next                    	Jump to next title
  num <0-9|+10>           	Sends numeric buttom 0-9 / '+10' to receiver
  off                     	Turns denon receiver off
  on                      	Turns denon receiver on
  online                  	Sets input source to Network and selects online music
  optical                 	Sets input source to Digital-In optical
  play/pause              	Pauses current playback
  play/pause              	Starts playback
  preset <+|->            	Zaps to previous / next preset
  previous                	Jump to previous title
  random                  	(does not seem to work)
  repeat                  	Toggles repeat option for playback
  rewind                  	Rewinds in current title
  right                   	Moves in current menu to the right
  sdb <on|off>            	SDB sound option on/off
  sdirect <on|off>        	Activates/deactivates s.direct input
  search                  	Enter search menu
  server                  	Sets input source to Network and selects server
  set-alarm <once|everyday> <hh:mm> <hh:mm> <analog1|analog2|optical|net|netusb|cd|cdusb|preset>[<preset no.>]	Configures alarm clock, e.g. set-alarm once 21:17 23:45 preset24
  sleep <0-255>           	Activates sleep mode with time in minutes
  standby <on|off>        	Sets auto-standby on/off
  stop                    	Stops current playback
  treble <+|->            	Increases / decreases treble level
  up                      	Moves in current menu up
  usb                     	Sets input source to CD and selects USB
  volume <0-60>           	Sets volume to value whish is between 0 and 60
 
```

## Example

### Turn receiver on
```
$ ./denon.py on
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <on>
 DONE: <on>
```

**Note:** For some reason this command always selects network as default input source instead of selecting the latest one.

### Select different input sources

Select FM-radio:
```
$ ./denon.py fm
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <fm>
 DONE: <fm>
``` 

Select CD which is one of the coax./S-PDIF inputs:
```
$ ./denon.py cd
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <cd>
 DONE: <cd>
``` 

Select optical which is one of the other digital inputs but optical:
```
$ ./denon.py optical
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <optical>
 DONE: <optical>
``` 

### Set master volume, mute and unmute

Set volume to value 8
```
$ ./denon.py vol 8
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <vol 8>
 DONE: <vol 8>
 
$ ./denon.py vol
  volume <0-60>           	Sets volume to value whish is between 0 and 60
  
$ ./denon.py mute on
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <mute on>
 DONE: <mute on>  

$ ./denon.py mute off
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <mute off>
 DONE: <mute off>
```


### Combine several commands
You can combine several commands like this:

1. Select FM/radio input source
2. Set volume 

```
$ ./denon.py fm vol 10
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <fm>
 DONE: <fm>
 INFO: Send command <vol 10>
 DONE: <vol 10>
``` 

### Turn receiver off
```
$ ./denon.py off
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <off>
 DONE: <off>
```

### Set FM radio and select preset

Simply select FM radio
```
$ ./denon.py fm
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <fm>
 DONE: <fm>
```

Zap presets backward and forward
```
$ ./denon.py preset -
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <preset ->
 DONE: <preset ->

$ ./denon.py preset +
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <preset +>
 DONE: <preset +>
```

Toggle stereo/mono mode
```
$ ./denon.py mode
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <mode>
 DONE: <mode>
```

### Sound settings

Increase / descrease bass level
```
$ ./denon.py bass +
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <bass +>
 DONE: <bass +>
 
$ ./denon.py bass -
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <bass ->
 DONE: <bass -> 
```

Enable SDB
```
$ ./denon.py sdb on
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <sdb on>
 DONE: <sdb on>
```

Set source-direct (s.direct)
```
$ ./denon.py sdirect on
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <sdirect on>
 DONE: <sdirect on>
```

### Set sleep timer and alarm clocks
Set sleep time to 90 minutes
```
$ ./denon.py sleep 90
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <sleep 90>
 DONE: <sleep 90>
```

Set everyday alarm clock
```
$ ./denon.py help set-alarm
  set-alarm <once|everyday> <hh:mm> <hh:mm> <analog1|analog2|optical|net|netusb|cd|cdusb|preset>[<preset no.>]	Configures alarm clock, e.g. set-alarm once 21:17 23:45 preset24
  
$ ./denon.py set-alarm everyday 06:30 07:45 preset24
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <set-alarm everyday 06:30 07:45 preset24>
 DONE: <set-alarm everyday 06:30 07:45 preset24>
```

Set activate everyday alarm clock
```
$ ./denon.py alarm everyday
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <alarm everyday>
 DONE: <alarm everyday>
```

### Display and other settings
Set display very bright and display some more information
```
$ ./denon.py dimmer high info info info
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <dimmer high>
 DONE: <dimmer high>
 INFO: Send command <info>
 DONE: <info>
 INFO: Send command <info>
 DONE: <info>
 INFO: Send command <info>
 DONE: <info>
```

Activate auto standby and turn display off
```
$ ./denon.py standby on dimmer off
 INFO: Serial device found </dev/ttyUSB0>
 INFO: Send command <standby on>
 DONE: <standby on>
 INFO: Send command <dimmer off>
 DONE: <dimmer off>
```

### Ask for help

Ask for help for a specific command:
```
$ ./denon.py help alarm
  alarm <off|on|once|everyday>	Activates / deactivates alarm clocks
```


## Limitations
Since I have only the Denon DRA-F109 receiver w/o any other components
I wasn't able to test everything but the basic remote control commands.

Therefore the following commands haven't been tested yet:


At this moment I haven't implemented the reading interfaces (RX) so that
this script isn't able to get information from receiver. 

In addition I believe (and hope) that there is more functionality like
setting the clock, reading station name, etc. Pleaes let me know if 
you have more information.