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

## First  boot

* Plug into ethernet and screen
* Raspberry pi setup will guide you through setting a username and password
* Configure other system settings: `sudo raspi-config`
  * Enable SSH
  * Set the hostname to be something sensible
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
* Put the contents of `networking/host/wlan0` in `/etc/network/interfaces.d/wlan0`
* Put the contents of `networking/host/wlan1` in `/etc/network/interfaces.d/wlan1`
* Put the contents of `networking/host/general` in `/etc/network/interfaces.d/general`
* `networking/host/wpa_supplicant-wlan1.conf` provides an example of how to specify wifi networks to connect to in `/etc/wpa_supplicant/wpa_supplicant-wlan1.conf`

* Install DHCP server: `sudo apt-get install isc-dhcp-server`
* Put contents of `networking/host/dhcpd.conf` in `/etc/dhcp/dhcpd.conf`
* Edit `/etc/default/isc-dhcp-server` and set INTERFACESv4="wlan0" and INTERFACESv6="wlan0"

* Install the AP server: `sudo apt-get install hostapd`
* Put the contents of `networking/host/hostapd.conf` in `/etc/hostapd/hostapd.conf`
* Change the # for the passphrase for the network
* Edit `/etc/default/hostapd` and point DAEMON_CONF to `/etc/hostapd/hostapd.conf`
* For some reason the service is masked in later raspbians:
`sudo systemctl unmask hostapd`
`sudo systemctl enable hostapd`
`sudo systemctl start hostapd`

* Reboot machine: `sudo shutdown -r now`

[Partial guide](https://learn.adafruit.com/setting-up-a-raspberry-pi-as-a-wifi-access-point/install-software)

[More](https://raspberrypihq.com/how-to-turn-a-raspberry-pi-into-a-wifi-router/)

[Angry, but helpful](https://tech.scargill.net/pi-zero-wi-fi-automatic-reconnect/)

[Probably the best docs](https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md)

### Client

* Run `ifconfig` to check you have wlan0
* Put the contents of `networking/client/wlan0` in `/etc/network/interfaces.d/wlan0`
* Put the contents of `networking/client/general` in `/etc/network/interfaces.d/general`
* Replace the two #'s with the SSID and passphrase for your robot's network, and also its static IP number
* **Append** the contents of `networking/host/dhcpcd.conf` to `/etc/dhcpcd.conf`

* Reboot machine: `sudo shutdown -r now`
