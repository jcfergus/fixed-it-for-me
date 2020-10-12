# fixed-it-for-me
A repository of things that fixed specific errors _for_ _me_.  

### Issues: 

#### [Spurious Error message from bind9 on Ubuntu 18.04.3](#bind9-ubuntu-18043)

* Date: 2020-02-09
* Error: 
  ```
  Feb 09 10:15:35 dc named[2913]: managed-keys-zone: Active key unexpectedly missing from .`  
  ```
  
  Fixed on Ubuntu 18.04.3 with: 
  ```bash
  cd /var/cache/named
  sudo rm managed-keys.bind*
  sudo service bind9 restart
  ```
  *Notes:* I _believe_ this error was harmless, but when something's not working right I like to try to get rid of as many error messages from the log as I can.
  
* *Tags:* ubuntu, 18.04.3, bionic beaver, named, bind9

#### [xsane failed to start scanner: invalid argument](#xsane-invalid-argument) 

* Date: 2020-07-14
* Error: 
```
xsane failed to start scanner: invalid argument
```

Fixed on ubuntu 20.04 with:
```bash
rm -r ~/.sane
```

* *Tags:* ubuntu, 20.04, xsane, invalid argument

#### [PomoDoneApp won't run on Ubuntu 20.04](#pomodone-ubuntu-2004)

* Date: 2020-05-07
* *Error:*
  ```jferg@quartz ~/Downloads $ pomodoneapp
  libtrace3/focal 3.0.21-1ubuntu3 amd64
  (pomodoneapp:38254): Pango-ERROR **: 08:34:06.746: Harfbuzz version too old (1.4.2)
  libtracker-control-2.0-dev/focal 2.3.4-1 amd64
  Trace/breakpoint trap (core dumped)
  jferg@quartz ~/Downloads $
  ```

  Fixed on Ubuntu 20.04 with:
  ```bash
  wget http://mirrors.kernel.org/ubuntu/pool/main/p/pango1.0/libpango-1.0-0_1.42.4-7_amd64.deb
  wget http://mirrors.kernel.org/ubuntu/pool/main/p/pango1.0/libpangocairo-1.0-0_1.42.4-7_amd64.deb
  wget http://mirrors.kernel.org/ubuntu/pool/main/p/pango1.0/libpangoft2-1.0-0_1.42.4-7_amd64.deb
  cd /opt/PomoDoneApp
  sudo dpkg -x libpango-1.0-0_1.42.4-7_amd64.deb .
  sudo dpkg -x libpangocairo-1.0-0_1.42.4-7_amd64.deb .
  sudo dpkg -x libpangoft2-1.0-0_1.42.4-7_amd64.deb .
  LD_LIBRARY_PATH=/opt/PomoDoneApp/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH ./pomodoneapp
  ```
  *Notes:*
  If you use gnome or KDE or another standard desktop you may want to edit the application descriptor in `/usr/share/applications` to include the LD_LIBRARY_PATH as well.

  Fixed on fedora 32 with:
  ```bash
  sudo dnf downgrade --releasever 30 pango-1.43.0-4.fc30.x86_64
  ```

* *Tags:* ubuntu, fedora, 20.04, focal fossa, pango, pomodone, pomodoneapp, harfbuzz

#### [Steam Client Exits Immediately on Ubuntu (with no error)](#steam-ubuntu-exits-immediately)

* Date: 2020-05-10
* *Error:"
  ```
  $ steam
  [2020-05-10 10:17:43] Nothing to do
  [2020-05-10 10:17:43] Verifying installation...
  [2020-05-10 10:17:43] Performing checksum verification of executable files
  [2020-05-10 10:17:44] Verification complete
  STEAM_RUNTIME_HEAVY: ./steam-runtime-heavy
  $
  ```

  On Ubuntu, the Steam client runs, installs updates, and then immediately exits.  For me, this was an issue with not having DRI3 enabled in Xorg.  To verify that this is your issue, run:
  ```
  $ grep "DRI" ~/.steam/root/error.log
  ``` 
  
  If it returns a line that looks something like: 
  ```
  Error: No DRI3 support
  ```
  then this is probably your problem.   
  
  Fixed on Ubuntu 20.04 by editing /etc/X11/xorg.conf to add the line: 
  ```
    Option      "DRI" "3"
  ```
  to the "Intel Graphics" device section:  
  ```
  Section "Device"
        Identifier  "Intel Graphics" 
        Driver      "intel"
        Option      "Backlight"  "intel_backlight"
        Option      "DRI" "3"
  EndSection
  ```
* *Tags:* steam, ubuntu, 20.04, focal fossa, dri, dri3, crash, immediate exit, intel graphics

