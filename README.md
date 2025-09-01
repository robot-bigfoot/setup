# setup

Setup for Raspberry Pi devices and in general

Loosely based on [previous robot version](https://github.com/AmoebaThree/RaspberryPiSetup/blob/master/README.md)

## Image

* Download Raspbian image, no need to unzip
  * [Image download page](https://www.raspberrypi.org/downloads/)
* Download [Etcher](https://www.balena.io/etcher/)
* Plug in SD card
* Etch the zip file directly to the SD card

[Source docs](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

[Other options](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)

## SSH setup

* On some controller machine, create ssh key: `ssh-keygen -t rsa`
* Copy it to the target machine:
  * `scp /home/username/.ssh/name_rsa.pub user@ip:/home/username/name_rsa.pub`
* SSH to the machine and add it to the authorised keys:
  * If not already there, make the file:
    * `mkdir  ~/.ssh`
    * `touch ~/.ssh/authorized_keys`
  * `cat /home/username/name_rsa.pub >> /home/username/.ssh/authorized_keys`
* Clean up the file in the home directory
* You may wish to make it the default SSH key on your client / controller device by adding the below to `.ssh/config`:
  * `IdentityFile /home/username/.ssh/name_rsa`

## First  boot

* Plug into ethernet and screen
* Raspberry pi setup will guide you through setting a username and password
* Configure other system settings: `sudo raspi-config`
  * Enable SSH
  * Set the hostname to be something sensible
  * Set the WiFi country to GB to unblock it
  * ([Details](http://www.raspberrypi-spy.co.uk/2012/05/enable-secure-shell-ssh-on-your-raspberry-pi/))

Once network is set up, update and install packages:

* `sudo apt update`
* `sudo apt install vim`
* `sudo apt autoremove`

If on a controller device with a Pi desktop environment, install vscode, then set it up and log in  (presumably with Github).

* `sudo apt install code`

On all devices, install and set up the Github CLI:

* [Install instructions](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian)
* `gh auth login` to log in
* Set up git:
  * `git config --global user.name "Your Name"`
  * `git config --global user.email "<youremail@yourdomain.com>"`

## Networking

### Host

For each file, check for any # that need replacing

* Put the contents of `networking/host/iptables.conf` in `/etc/iptables.conf`
* For each wifi you want to connect to, create a version of  `networking/host/WiFiNetwork.nmconnection` in `/etc/NetworkManager/system-connections`
  * You **MUST** chmod these to 600 or NetworkManager will ignore them
* For the host network, put  the example from `networking/host/WiFiHost.nmconnection` in `/etc/NetworkManager/system-connections`
  * You **MUST** chmod these to 600 or NetworkManager will ignore them

Sometimes the internet won't be forwarded, NetworkManager has probably failed to create the firewall rules for you (the output of `sudo nft list ruleset` is empty). In this case, bring your host WiFi back up with `sudo nmcli connection up #` (where # is the connection name you configured) and check the ruleset list again and you should be good.

Reboot machine: `sudo shutdown -r now`

NetworkManager instead of old stack kinda documented [here](https://raspberrypi.stackexchange.com/a/145594)

[raspi-config docs](https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md)

### Client

* Run `ifconfig` to check you have wlan0
* For each wifi you want to connect to, create a version of  `networking/client/WiFiNetwork.nmconnection` in `/etc/NetworkManager/system-connections`
  * You **MUST** chmod these to 600 or NetworkManager will ignore them

Reboot machine: `sudo shutdown -r now`
