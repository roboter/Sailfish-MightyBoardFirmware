
The "Generation3" firmware is the firmware used by MakerBot's Generation 3 and Generation 4 electronics.

# Building the firmware from source
## Getting ready to build
To build and install the firmware, you'll need these tools:
    
- The avr-gcc compiler and library
    
- avrdude, a tool for uploading hex files
    
- scons, a build tool

### Fedora
If you're running Fedora, you can install the whole toolchain with one command (as root):

```bash
yum install avr-gcc avr-libc avrdude scons git
```


### Ubuntu/Debian
If you're running some flavor of Debian, you can easily install all three from the standard repositories:

```bash
apt-get install gcc-avr avr-libc avrdude scons
```

If you don't already have Git installed, the package git-core should provide it, at least in Ubuntu.
```bash
apt-get install git-core
```
should do it.

### OS X    
- install jmil's new [MacAVR](https://github.com/downloads/jmil/MacAVR/AVR_0018.pkg) package, which installs the latest avr-gcc and avrdude from Arduino at /usr/local/avr and updates your PATH in ~/.bash_profile to point there. If MacAVR doesn't work for you, you can install manually by using the versions of avr-gcc and avrdude that are bundled with Arduino. See the discussion [here](https://groups.google.com/group/gen3-firmware-development/browse_thread/thread/f3b78dace01bc7be)
for using avr tools from [Arduino](https://arduino.googlecode.com/files/arduino-0018.dmg)
-  You will need to build scons yourself: First Install [MacPorts](https://www.macports.org/install.php), then  [run the command listed on Darwinports for building scons](https://scons.darwinports.com/):
        

```bash
sudo port install scons
```
It will take a while (~30 min) for MacPorts to build and install SCons along with all of its dependencies.

That's everything you need. After installation (and a reboot), check that each package was installed correctly in a new Terminal window with the "which" command:

```bash
% which avr-gcc
/usr/local/avr/bin/avr-gcc
% which avrdude
/usr/local/avr/bin/avrdude
% which scons
/opt/local/bin/scons
```
Note: As of 2010-04-19 avr-gcc and avrdude on Darwin ports are not able to build the v2 software. Use instructions above instead.  
Deprecated build instructions for avr-gcc and avrdude:  
[AVR-GCC on Darwin ports](https://avr-gcc.darwinports.com/)  
[avrdude on Darwin ports](https://avrdude.darwinports.com/)

### Windows
If you're using Windows, you can use the versions of avr-gcc and avrdude that are bundled with Arduino, or install them yourself:

- [AVR-GCC and avrdude for Windows](https://winavr.sourceforge.net/)
    
- [SCons site](https://www.scons.org/)
        

## Get the source
There are two ways to do this: download a tagged zip/tarball, or pull down the latest source.

### Getting the latest source
You can get the latest code from the [G3Firmware github repository](https://github.com/makerbot/G3Firmware/). First install[ git](https://git-scm.com/download), and then clone the tree:

```bash
git clone git://github.com/makerbot/G3Firmware.git
```
If you're new to git, definitely check out the [tutorials](https://git-scm.com/documentation).

If you've already cloned the tree in the past, you can update it by changing to the G3Firmware directory and typing:

```bash
git pull
```
### Getting a tagged zip or tarball
If you don't have git installed or don't want to install it, you can get a tagged build from the [GitHub download page](https://github.com/makerbot/G3Firmware/downloads).

## Running the build
Once avr-g++, scons, and avrdude are in your path, you're pretty much ready to go. Switch to the

```bash
G3Firmware/firmware
```
directory and type:
```bash
scons
```

to build the motherboard firmware. If you have a cable hooked up to the board, you can upload the firmware with the scons script:
```bash
scons port=/dev/NAME_OF_YOUR_SERIAL_DEVICE upload
```

You'll have to manually press the reset button on the board as you run the upload.

If you are trying to build firmwares for motherboards other than the default Thing-O-Matic board, you can define the platform you want to compile for with:

```bash
# Generation 3 RepRap Motherboard v1.2
scons platform=rrmbv12
# Generation 4 MakerBot Motherboard w/ Arduino Mega
scons platform=mb24
# Generation 4 MakerBot Motherboard w/ Mega 2560
scons platform=mb24-2560
```

You'll also have to define the platform when you upload to a board.

Building and uploading the extruder firmware is similar. Just specify an extruder controller as the platform:

```bash
scons platform=ecv22
scons platform=ecv22 port=/dev/NAME_OF_YOUR_SERIAL_DEVICE upload
```

Like the motherboard, if you need to upload to any extruder board that is not the default Cupcake board you need to define the platform:

```bash
# Generation 3 extruder controller
scons platform=ecv22
# Generation 4 extruder controller
scons platform=ecv34
```

You can also define whether or not the board will be using relays or stepper extruders using the "relays=false" or "stepper=true" tags.

Don't forget you'll have to manually press the reset button on the board as you run the upload.

Remember, to upload to an extruder board, you must have your USB-serial cable connected correctly to the extruder board itself! You cannot program an extruder board via the motherboard.
