Watchdog1337pitft README v.1.0
=========================
This is a fork of http://github.com/chr1573r/watchdog1337

Written by Christer Jonassen - Cj Designs
http://csdnserver.com

Watchdog1337 is licensed under CC BY-NC-SA 3.0.
Watchdog1337pitft is licensed under CC BY-NC-SA 3.0.
(check LICENCE file or http://creativecommons.org/licenses/by-nc-sa/3.0/ for details.)


What is Watchdog1337?
---------------------

Watchdog1337 is a simple network monitoring script written in bash. 
It pings the desired hosts in a fixed time interval and displays the status.


What is Watchdog1337pitft?
---------------------
Watchdog1337pitft is a WD1337 fork specificly tailored to fit WD1337 into a 53x21 terminal,
which is the native framebuffer terminal resolution for Raspberry Pi 320x240px TFT touch display shields


Install instructions
----------------------

Providing that you have downloaded and unzipped Watchdog1337pitft on your computer:

### 1. Adding hosts
Add the hosts you want to monitor to the file named "hosts.lst" in the following format:
`<name>:<location>:<ip>`
One host per line.


### 2. Check settings
Revise default options set in `settings.cfg` and change them if you need to.
You can also add a custom command in `settings.cfg` that will be executed after each ping round.
Read more about the custom cmd further below)

### 3. Permit and execute
Give watchdog1337pitft.sh permission to run by executing the following command:
`chmod +x watchdog1337pitft.sh`

That's pretty much it! You can now start Watchdog1337 by executing the following:
`./watchdog1337pitft.sh`


How does it work?
-----------------

Watchdog1337pitft reads host information from the file hosts.lst and settings from settings.cfg. 
It then pings all the hosts from hosts.lst in order and returns the exit code of the ping.

If a host does not respond, the host is highlighted in red until it responds or are removed from hosts.lst. 

After pinging all hosts, Watchdog1337pitft provides a short summary
and waits a specified number of seconds* before pinging them again.
(* Set in settings.cfg or directly upon execution, e.g `./watchdog1337pitft.sh 300` for 5 minute intervals) 

Watchdog1337pitft continues to run until interrupted by `Ctrl-C` or killed otherwise.
You can also instruct it to exit after one run by setting RUN_ONCE to 1 in settings.cfg. 
 

Custom command execution
------------------------

As stated previously, Watchdog1337pitft can be set to execute a custom command after each ping round.
The complete contents of the variable CUSTOMCMD is executed as: `eval "$CUSTOMCMD"`
Working example that can be set in settings.cfg:
`CUSTOMCMD='echo $HOSTS > /tmp/totalhosts.txt'        #This writes the total number of hosts to the file /tmp/totalhosts.txt`

Make sure you get the formatting and necessary escapes right, otherwise Watchdog1337pitft won't work.
Study the Watchdog1337pitft.sh sourcecode to find variables
that might be interesting in combination with CUSTOMCMD


Technical details:
------------------

Written in bash, uses ping to determine host status. 
Besides bash, the following common binaries are used:
`ping`, `uptime`, `hostname`, `tput`, `date`, `sleep`, `awk`, `tail`, `cut`. 
Should run on the most common Linux distros, 
but it has only been tested properly on Raspian


Limitations
-----------

There are no limitation on the number of hosts, but keep in mind that hosts are pinged one at a time,
and thus you won't know the status of the first host again before the last host is pinged (and after the set wait period)

Each host consumes one(1) line in the console output,
so keep this in mind if you want to display all hosts at the same time.

While only tested briefly, Watchdog1337pitft does not function properly on
Mac OS X 10.6 or the FreeBSD distro pfSense. (When executed natively of course, not ssh sessions)
It should work out-of-the-box on GNU/Linux distros.

If you somehow should get an empty line or partially missing text when the last host is pinged,
try adding a blank line at the end of `hosts.lst`.
 
Also remember that Watchdog1337pitft is very simple and only determines host status by ping. 
It can not tell if apache is denying connections on your webserver, unfortunately ;)