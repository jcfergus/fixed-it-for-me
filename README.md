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
  cd /tmp
  wget http://launchpadlibrarian.net/438303557/libpango-1.0-0_1.42.4-7_amd64.deb
  wget http://launchpadlibrarian.net/438303558/libpangocairo-1.0-0_1.42.4-7_amd64.deb
  wget http://launchpadlibrarian.net/438303559/libpangoft2-1.0-0_1.42.4-7_amd64.deb
  cd /opt/PomoDoneApp
  sudo dpkg -x /tmp/libpango-1.0-0_1.42.4-7_amd64.deb .
  sudo dpkg -x /tmp/libpangocairo-1.0-0_1.42.4-7_amd64.deb .
  sudo dpkg -x /tmp/libpangoft2-1.0-0_1.42.4-7_amd64.deb .
  LD_LIBRARY_PATH=/opt/PomoDoneApp/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH ./pomodoneapp
  ```
  *Notes:*
  If you use gnome or KDE or another standard desktop you may want to edit the application descriptor in `/usr/share/applications` to include the LD_LIBRARY_PATH as well.


  Fixed on fedora 32 with:
  ```bash
  sudo dnf downgrade --releasever 30 pango-1.43.0-4.fc30.x86_64
  ```

* *Tags:* ubuntu, fedora, 20.04, focal fossa, pango, pomodone, pomodoneapp, harfbuzz
* *With contributions from*: [Marcin Maruszewski](https://github.com/marcinmaruszewski), [damani42](https://github.com/damani42), [Carl-Erik Kopseng](https://github.com/fatso83) 

#### [PomoDoneApp won't run on Parrot OS ](#pomodone-debian-2004)

* Date: 2021-01-05
* *Error:*
  ```bash
  himel@b1ack_c0de ~/Downloads $ pomodoneapp
  libtrace3/focal 3.0.21-1ubuntu3 amd64
  (pomodoneapp:38254): Pango-ERROR **: 08:34:06.746: Harfbuzz version too old (1.4.2)
  libtracker-control-2.0-dev/focal 2.3.4-1 amd64
  Trace/breakpoint trap (core dumped)
  himel@b1ack_c0de ~/Downloads $
  ```
  Fixed on Debian 10(Parrot os)  with:
  ```bash
  sudo dpkg -i libpango-1.0-0_1.42.4-8_deb10u1_amd64.deb
  wget http://ftp.br.debian.org/debian/pool/main/p/pango1.0/libpango-1.0-0_1.42.4-8~deb10u1_amd64.deb
  wget http://ftp.cn.debian.org/debian/pool/main/p/pango1.0/libpangocairo-1.0-0_1.42.4-8~deb10u1_amd64.deb
  wget http://ftp.cn.debian.org/debian/pool/main/p/pango1.0/libpangoft2-1.0-0_1.42.4-8~deb10u1_amd64.deb
  wget http://ftp.cn.debian.org/debian/pool/main/p/pango1.0/libpangoxft-1.0-0_1.42.4-8~deb10u1_amd64.deb

  sudo dpkg -i libpango-1.0-0_1.42.4-8~deb10u1_amd64.deb libpangocairo-1.0-0_1.42.4-8~deb10u1_amd64.deb libpangoft2-1.0-0_1.42.4-8~deb10u1_amd64.deb libpangoxft-1.0-0_1.42.4-8~deb10u1_amd64.deb
  ```

  *Notes:*
  You may get downgrade warning. Don't update those packages. To prevent their update you not to follow this steps:
  ```bash
  $ apt list --upgradable 
  Listing... Done
  libpango-1.0-0/rolling,rolling 1.46.2-3 amd64 [upgradable from: 1.42.4-8~deb10u1]
  libpangocairo-1.0-0/rolling,rolling 1.46.2-3 amd64 [upgradable from: 1.42.4-8~deb10u1]
  libpangoft2-1.0-0/rolling,rolling 1.46.2-3 amd64 [upgradable from: 1.42.4-8~deb10u1]

  $ sudo apt-mark hold libpango-1.0-0:amd64 libpangocairo-1.0-0:amd64 libpangoft2-1.0-0:amd64
  libpango-1.0-0 set on hold.
  libpangocairo-1.0-0 set on hold.
  libpangoft2-1.0-0 set on hold.
  ```
  You can unhold any time by using 
  ```bash
  udo apt-mark unhold package_name
  ```
  If you are using other Architecture then goto https://packages.debian.org/buster/gir1.2-pango-1.0 then find out the required packages. 

* *Tags:* parrot, debian 10, buster, pango, pomodone, pomodoneapp, harfbuzz
* *Contributed by*: [Habibu-R-ahman](https://github.com/Habibu-R-ahman)

#### [ChromeOS (Brunch) on Dell Laptop - Keyboard stops working on resume from suspend](#chromeos-keyboard-stops-working-after-suspend)

* Date 2020-12-07
* No specific error message, but symptom is that the keyboard stops working after resuming from suspend or sleep on a Dell laptop running ChromeOS via [Brunch](https://github.com/sebanc/brunch).  

  *Notes:* Appears to be caused by an issue in the i8042 keyboard driver.
  
  To fix, you will need to press Ctrl-Alt-F2 to enter the Linux console.  (This will still work even though the keyboard is not responding in the GUI.)  Then:
  1. Log in with the username `chronos`.
  2. Type `/usr/sbin/edit-grub-config` at the commandline.  
  3. Edit the grub configuration to append a space and then `i8042.reset=1` to the line that starts with `linux`. 
  4. Press Ctrl-O to save, Ctrl-X to quit.
  5. Press Ctrl-Alt-F1 to return to the GUI, use the mouse to navigate to the menu, and shut down the laptop.  
  6. Power it back on and you should be set.  

* *Tags:* chromeos, brunch, dell, keyboard, i8042, suspend, sleep, stops working

#### [Raspbian `apt-get update` gives error (actually a warning) about "Repository ... changed its 'Suite'"](#raspbian-changed-suite)

* Date: 2021-12-18
* *Error:*
  ```
  pi@octopi:~ $ sudo apt-get update
  Hit:1 http://raspbian.raspberrypi.org/raspbian buster InRelease
  Get:2 http://archive.raspberrypi.org/debian buster InRelease [32.6 kB]
  Reading package lists... Done
  E: Repository 'http://archive.raspberrypi.org/debian buster InRelease' changed its 'Suite' value from 'testing' to 'oldstable'
  N: This must be accepted explicitly before updates for this repository can be applied. See apt-secure(8) manpage for details.
  $
  ```
  
  It _is_ just a warning, and shouldn't stop things from working, but it's annoying.  Since the `apt-secure(8)` manpage is nigh on useless as far as actually
  resolving the issue, the following command will take care of it:
  
  ```
  sudo apt-get --allow-releaseinfo-change update
  ```
  
  Thanks to `/u/beer118` for this reddit post where I found this solution: https://www.reddit.com/r/debian/comments/ca3se6/for_people_who_gets_this_error_inrelease_changed/
  
* *Tags:* raspbian, buster, suite, apt-get, update, apt-secure

#### [Google Meet on Android Refuses to use Bluetooth Headset](#google-meet-android-bluetooth)

* Date: 2022-01-10
* *Error:* Google Meet on Android will not send audio to Bluetooth, despite having Bluetooth selected as the audio device within Meet. 

  I'm putting this here because I was unable to turn up this answer with a fair amount of searching.  To fix this issue, all you need to do is re-pair the Bluetooth device, making sure to select "Share contacts with this device." in the pairing dialog.  (I don't usually check that box for $REASONS.)  I don't understand exactly why, but this has fixed the issue for me on two separate Bluetooth devices (headset and vehicle audio).  
  
* *Tags:* bluetooth, google meet, android, pairing

#### [Terraform Fails to Update Managed Certificate for lb_http-created Load Balancer](#terraform-lb_http-certificate-update-failed)

* Date: 2022-01-12
* *Error:* 
  ```
  Error when reading or editing ManagedSslCertificate: googleapi: Error 400: The ssl_certificate resource ...  is already being used by ... , resourceInUseByAnotherResource`
  ```
  
  This came up while trying to update a Google Cloud Load Balancer created using the [lb-http](https://github.com/terraform-google-modules/terraform-google-lb-http) module to add another hostname to the load balancer.  Searching turns up several instances of people having the problem but I couldn't find a succinct answer to it anywhere using the `lb-http` module.  (The documentation alludes to it but you have to know what you're looking for.)  It's actually a pretty simple fix: 
  
  1. Make sure you're using version 6.0.1 or better of the `lb-http` module.
  ```
    version = "~> 6.1"
  ```
  2. Set `random_certificate_suffix` on the module inputs:
  ```
    random_certificate_suffix = true
  ```

  This will trigger a creation of a new certificate with a random suffix instead of trying to replace the existing certificate!
  
* *Tags:* terraform, google cloud, gcp, lb-http, managedsslcertificate, ssl_certificate, resourceinusebyanotherresource
  

#### [Steam Client Exits Immediately on Ubuntu (with no error)](#steam-ubuntu-exits-immediately) 

* Date: 2020-05-10
* *Error:*
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

