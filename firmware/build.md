
The "Generation3" firmware is the firmware used by MakerBot's Generation 3 and
Generation 4 electronics.

# <span>Building the firmware from source</span>
## <span>Getting ready to build</span>
To build and install the firmware, you'll need these tools:

    
- The avr-gcc compiler and library
    
- avrdude, a tool for uploading hex files
    
- scons, a build tool

### <span>Fedora</span>
    If you're running Fedora, you can install the whole toolchain with one command
    (as root):

<div class="code">
    <pre><code>yum install avr-gcc avr-libc avrdude scons git</code>
</pre>
</div>
### <span>Ubuntu/Debian</span>
    If you're running some flavor of Debian, you can easily install all three from
    the standard repositories:

<div class="code">
    <pre><code>apt-get install gcc-avr avr-libc avrdude scons</code>
</pre>
</div>
<br>
If you don't already have Git installed, the package git-core should provide it,
at least in Ubuntu.
<div class="code">
    <pre><code>apt-get install git-core</code>
</pre>
</div>
<br>
should do it.
### <span>OS X</span>

    
- 
            install jmil's new
            [MacAVR](https://web.archive.org/web/20111102132711/http://github.com/downloads/jmil/MacAVR/AVR_0018.pkg)
            package, which installs the latest avr-gcc and avrdude from Arduino at
            /usr/local/avr and updates your PATH in ~/.bash_profile to point there. If
            MacAVR doesn't work for you, you can install manually by using the versions
            of avr-gcc and avrdude that are bundled with Arduino. See the discussion
            [here](https://web.archive.org/web/20111102132711/http://groups.google.com/group/gen3-firmware-development/browse_thread/thread/f3b78dace01bc7be)
            for using avr tools from
            [Arduino
                0018](https://web.archive.org/web/20111102132711/http://arduino.googlecode.com/files/arduino-0018.dmg)
        


    
- 
            You will need to build scons yourself: First
            [Install MacPorts](https://web.archive.org/web/20111102132711/http://www.macports.org/install.php),
            then
            [run the command listed on
                Darwinports for building scons](https://web.archive.org/web/20111102132711/http://scons.darwinports.com/):
        

<div class="code">
    <pre><code>sudo port install scons</code>
</pre>
</div>
    It will take a while (~30 min) for MacPorts to build and install SCons along
    with all of its dependencies.

    That's everything you need. After installation (and a reboot), check that each
    package was installed correctly in a new Terminal window with the "which"
    command:

<div class="code">
    <pre><code>% which avr-gcc
/usr/local/avr/bin/avr-gcc
% which avrdude
/usr/local/avr/bin/avrdude
% which scons
/opt/local/bin/scons</code>
</pre>
</div>
    Note: As of 2010-04-19 avr-gcc and avrdude on Darwin ports are not able to
    build the v2 software. Use instructions above instead.  
    Deprecated build instructions for avr-gcc and avrdude:  
    <span style="text-decoration-line: line-through;">[AVR-GCC on Darwin
            ports](https://web.archive.org/web/20111102132711/http://avr-gcc.darwinports.com/)</span>  
    <span style="text-decoration-line: line-through;">[avrdude on Darwin
            ports](https://web.archive.org/web/20111102132711/http://avrdude.darwinports.com/)</span>

### <span>Windows</span>
    If you're using Windows, you can use the versions of avr-gcc and avrdude that
    are bundled with Arduino, or install them yourself:


    
- 
            [AVR-GCC and avrdude for
                Windows](https://web.archive.org/web/20111102132711/http://winavr.sourceforge.net/)
        
    
- 
            [SCons site](https://web.archive.org/web/20111102132711/http://www.scons.org/)
        

## <span>Get the source</span>
    There are two ways to do this: download a tagged zip/tarball, or pull down the
    latest source.

### <span>Getting the latest source</span>
    You can get the latest code from the
    [G3Firmware github
        repository](https://web.archive.org/web/20111102132711/http://github.com/makerbot/G3Firmware/). First
    [install git](https://web.archive.org/web/20111102132711/http://git-scm.com/download), and then clone the
    tree:

<div class="code">
    <pre><code>git clone git://github.com/makerbot/G3Firmware.git</code>
</pre>
</div>
    If you're new to git, definitely check out the
    [tutorials](https://web.archive.org/web/20111102132711/http://git-scm.com/documentation).

    If you've already cloned the tree in the past, you can update it by changing
    to the G3Firmware directory and typing:

<div class="code">
    <pre><code>git pull</code>
</pre>
</div>
### <span>Getting a tagged zip or tarball</span>
    If you don't have git installed or don't want to install it, you can get a
    tagged build from the
    [GitHub download
        page](https://web.archive.org/web/20111102132711/http://github.com/makerbot/G3Firmware/downloads).

## <span>Running the build</span>
    Once avr-g++, scons, and avrdude are in your path, you're pretty much ready to
    go. Switch to the

<div class="code">
    <pre><code>G3Firmware/firmware</code>
</pre>
</div>
<br>
directory and type:
<div class="code">
    <pre><code>scons</code>
</pre>
</div>
<br>
to build the motherboard firmware. If you have a cable hooked up to the board,
you can upload the firmware with the scons script:
<div class="code">
    <pre><code>scons port=/dev/NAME_OF_YOUR_SERIAL_DEVICE upload</code>
</pre>
</div>
<br>
You'll have to manually press the reset button on the board as you run the
upload.

    If you are trying to build firmwares for motherboards other than the default
    Thing-O-Matic board, you can define the platform you want to compile for with:

<div class="code">
    <pre><code># Generation 3 RepRap Motherboard v1.2
scons platform=rrmbv12
# Generation 4 MakerBot Motherboard w/ Arduino Mega
scons platform=mb24
# Generation 4 MakerBot Motherboard w/ Mega 2560
scons platform=mb24-2560</code>
</pre>
</div>
You'll also have to define the platform when you upload to a board.

    Building and uploading the extruder firmware is similar. Just specify an
    extruder controller as the platform:

<div class="code">
    <pre><code>scons platform=ecv22
scons platform=ecv22 port=/dev/NAME_OF_YOUR_SERIAL_DEVICE upload</code>
</pre>
</div>
    Like the motherboard, if you need to upload to any extruder board that is not
    the default Cupcake board you need to define the platform:

<div class="code">
    <pre><code># Generation 3 extruder controller
scons platform=ecv22
# Generation 4 extruder controller
scons platform=ecv34</code>
</pre>
</div>
    You can also define whether or not the board will be using relays or stepper
    extruders using the "relays=false" or "stepper=true" tags.

    Don't forget you'll have to manually press the reset button on the board as
    you run the upload.

    Remember, to upload to an extruder board, you must have your USB-serial cable
    connected correctly to the extruder board itself! You cannot program an
    extruder board via the motherboard.
