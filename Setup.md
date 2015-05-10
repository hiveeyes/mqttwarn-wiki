## Requirements

You'll need at least the following components:

* Python 2.x (tested with 2.6 and 2.7)
* An MQTT broker (e.g. [Mosquitto](http://mosquitto.org) or [Mosca](http://www.mosca.io/))
* The [Paho](http://www.eclipse.org/paho/) Python module: `pip install paho-mqtt`

## Installation

1. Clone this repository into a fresh directory.
  `git clone https://github.com/jpmens/mqttwarn.git`
2. Copy `mqttwarn.ini.sample` to `mqttwarn.ini` and edit to your taste
3. Install the prerequisite Python modules for the services you want to use
4. Launch `mqttwarn.py`

I recommend you use [Supervisor](http://jpmens.net/2014/02/13/in-my-toolbox-supervisord/) for running this.

### Installation on a Mac

This paragraph explains how to install and use mqttwarn without needing any developer tools like git. It presumes some experience with OS X Terminal. By installing it to `~/opt` instead op `/opt` you will not need sudo rights to use it

Download and install from original repo through terminal:

    cd ~/Downloads
    curl -LO https://github.com/jpmens/mqttwarn/archive/master.zip
    unzip master.zip
    mkdir ~/opt
    cp -R mqttwarn-master ~/opt/mqttwarn
    rm -R mqttwarn-master
    rm master.zip
    cd ~/opt/mqttwarn

Copy `mqttwarn.ini.sample` to `mqttwarn.ini` and edit to your taste

    cp mqttwarn.ini.sample mqttwarn.ini

Install the [Paho](http://www.eclipse.org/paho/) Python module (you need sudo)

    sudo pip install paho-mqtt

Optionally, install the prerequisite Python modules for the services you want to use

Launch `mqttwarn.py`

    chmod u+x mqttwarn.py
    ./mqttwarn.py