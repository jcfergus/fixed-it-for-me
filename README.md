# fixed-it-for-me
A repository of things that fixed specific errors _for_ _me_.  

Issues: 

* `Feb 09 10:15:35 dc named[2913]: managed-keys-zone: Active key unexpectedly missing from .`  
  Fixed on Ubuntu 18.04.3 with: 
  `cd /var/cache/named; rm managed-keys.bind*; service bind9 restart`
  
  I _believe_ this error was harmless, but when something's not working right I like to try to get rid of as many error messages from the log as I can.
