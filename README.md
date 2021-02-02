# Setting up a Raspberry Pi as wireless access point, that hosts a shiny server

## Note

The majority of this script is based on the work from [pjaselin](https://github.com/pjaselin/ShinyServer_On_RaspberryPi). This includes:
- Handling system library dependencies for installing and running a Shiny Server
- Installing all R packages required for Shiny Server to start and index.html to successfully open
- Editing external/node/install-node.sh to obtain the correct NodeJS installation for a Raspberry Pi
- Building Shiny Server via packaging/make-package.sh
- Configuring Shiny Server, sets up initial server applications, and places all the required system directories
- Resolving Pandoc issues arising from Shiny Server using it's packaged Pandoc distribution by removing this directory and giving the system-installed version precedence

Additional tasks, that are solved in the script, are:
- Installation of additional R-packages (shinydashboard, shinydashboardPlus, png, dygraphs)
- Setting up the Pi as a wireless access point; devices can connect directly to the Pi in order to see the shiny-App hosted by the Pi, without the need for a network connection

## Contents
- [Warnings](#Warnings)
- [Installation with Stable R](#Installation-with-Stable-R)
- [Installation with Backport R](#Installation-with-Backport-R)
- [Uninstall Shiny-Server and R](#Uninstall-Shiny-Server-and-R)
- [Get your App onto the shiny server](#Get-your-App-onto-the-shiny-server)
- [Start/Stop the shiny server](#Start/Stop-the-shiny-server)
- [Connect to the Pi and see the App](#Connect-to-the-Pi-and-see-the-App)



## Warnings
These scripts are provided "as is" with no warranty of any kind. As such, users should read the script to ensure they are confident in its integrity. The script is well contained and should not interfere with anything on the RPi beyond Shiny Server-related tasks. These have been tested and are known to work as of January 24, 2021 (forked script by [pjaselin](https://github.com/pjaselin/ShinyServer_On_RaspberryPi)) and February 02, 2021 (complete script).

## Installation with Stable R

The provided Stable_RPiShinyServer.sh script will install the latest Shiny Server distribution along with the stable R version via the following command (N.B. the script takes awhile to run due to the R library installations):
```bash
wget -O - https://raw.githubusercontent.com/langdav/ShinyServer_On_RaspberryPi/master/StableInstall_RPiShinyServer.sh | bash
```

## Installation with Backport R
Warning: you may attempt this at your own risk. If you already have R installed, this could remove all previously installed R libraries. If you would like to try using the latest (or later) version of R, you may follow these instructions to point the Raspberry Pi to the required mirrors (N.B. if you would like to upgrade your Raspberry Pi to a newer OS such as the Buster-based version, see here: https://www.raspberrypi.org/documentation/raspbian/updating.md).


### Add Secure Apt Key
Sourced from: https://cloud.r-project.org/bin/linux/debian/#secure-apt
```bash
sudo apt-get install dirmngr --install-recommends
sudo apt-key adv --keyserver keys.gnupg.net --recv-key 'E19F5F87128899B192B1A2C2AD5F960A256A04AF'
sudo apt-get -y update && sudo apt-get -y upgrade
```

### Add the Branch-Specific Mirror
Sourced from: https://cloud.r-project.org/bin/linux/debian/#supported-branches

#### For Buster & R 4
```bash
echo 'deb http://cloud.r-project.org/bin/linux/debian buster-cran40/' | sudo tee --append /etc/apt/sources.list
```

#### For Buster & R 3.6.3
```bash
echo 'deb http://cloud.r-project.org/bin/linux/debian buster-cran35/' | sudo tee --append /etc/apt/sources.list
```

#### For Stretch & R 3.6.3
```bash
echo 'deb http://cloud.r-project.org/bin/linux/debian stretch-cran35/' | sudo tee --append /etc/apt/sources.list
```

#### Finally Install Shiny Server and R
```bash
wget -O - https://raw.githubusercontent.com/pjaselin/ShinyServer_On_RaspberryPi/master/StableInstall_RPiShinyServer.sh | bash
```

## Uninstall Shiny Server and R
To remove Shiny Server AND R, please run the following command to execute the Uninstall_RPiShinyServer.sh script:
```bash
wget -O - https://raw.githubusercontent.com/pjaselin/ShinyServer_On_RaspberryPi/master/Uninstall_RPiShinyServer.sh | bash
```
If you would only like to remove Shiny Server, please look into this script and only use what you need.

## Get your App onto the shiny server

### Copy App data by using a removable thumb drive
replace NAME_OF_REMOVABLE_DRIVE with name of your drive (without '')
replace NAME_OF_APP with name of your folder (without '')
```bash
sudo cp -R /media/pi/'NAME_OF_REMOVABLE_DRIVE'/'NAME_OF_APP' /srv/shiny-server
```

## Start/Stop the shiny server
### Starting the server
```bash
sudo systemctl start shiny-server
```
You need to enter you user password

### Stopping the server
```bash
sudo systemctl stop shiny-server
```
You need to enter you user password


## Connect to the Pi and see the App
- Connect to the Pi with any wireless device (SSID: shinyPi; PW: shinyTestPWRPi (the password will be removed in the final version)).
- Open your internet browser and go to "192.168.4.1:3838/NAME_OF_APP/" (replace "NAME_OF_APP" with the name of your app)