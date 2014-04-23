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