# fixed-it-for-me
A repository of things that fixed specific errors _for_ _me_.  

### Issues: 

#### Spurious Error message from bind9 on Ubuntu 18.04.3 [#bind9-ubuntu-18043]

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

#### PomoDoneApp won't run on Ubuntu 20.04 [#pomodone-ubuntu-2004]

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
  sudo dpkg -x libpangocairo-1.0-0_1.42.4-7_amd64.deb
  sudo dpkg -x libpangoft2-1.0-0_1.42.4-7_amd64.deb
  LD_LIBRARY_PATH=/opt/PomoDoneApp/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH ./pomodoneapp
  ```
  *Notes:*
  If you use gnome or KDE or another standard desktop you may want to edit the application descriptor in `/usr/share/applications` to include the LD_LIBRARY_PATH as well.

