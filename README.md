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
