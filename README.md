# **Kelvin** - The hue bot

[![Build Status](https://travis-ci.org/stefanwichmann/kelvin.svg?branch=master)](https://travis-ci.org/stefanwichmann/kelvin)
[![Go Report Card](https://goreportcard.com/badge/github.com/stefanwichmann/kelvin)](https://goreportcard.com/report/github.com/stefanwichmann/kelvin)

# Meet Kelvin
Kelvin is a little helper bot who will automate the lights in your house. Its job is to adjust the color temperature and brightness in your home based on your local sunrise and sunset times and custom intervals defined by you. Think of it as [f.lux](https://justgetflux.com/) or Apple's Night Shift for your home.

Imagine your lights shine in an energetic but not to bright blue color to get you started in the early morning. On sunrise your lights will change to a more natural color temperature to reflect the sunlight outside. On sunset they will slowly fade to a warmer and softer color scheme perfectly suited to Netflix and chill. When it's time to go to bed Kelvin will reduce the intensity even more to get you into a sleepy mood. It will keep this reduced setting through the night so you don't get blinded by bright lights if you have to get up at night...

# Features
- Adjust the color temperature and brightness of your lights based on the local sunrise and sunset times
- Define a fine grained daily schedule to fit your personal needs throughout the day
- Define a default startup color for your lights
- Gradual light transitions you won't even notice
- Works with smart switches as well as conventional switches
- Respects manual light changes until a light is switched off and on again
- Auto upgrade to seamlessly deliver improvements to you
- Small, self contained binary with sane defaults and no dependencies to get you started right away
- Free and open source


# Getting started
If you want to give Kelvin a try, there are some things you will need to benefit from its services:
- [ ] Supported **Philips Hue** (or compatible) lights
- [ ] A configured **Philips Hue** bridge
- [ ] A permanently running computer connected to your network (See [Raspberry Pi](#raspberry-pi))

Got all these? Great, let's get started!

# Installation
1. Download the latest version of Kelvin from the [Releases](https://github.com/stefanwichmann/kelvin/releases) page.
2. Extract the Kelvin archive.
3. Start Kelvin by double-clicking `kelvin.exe` on windows or by typing `./kelvin` in your terminal on macOS, Linux and other Unix-based systems.
   You should see an output similar to the following snippet:
   ```
   2017/03/22 10:45:41 Kelvin v0.0.7 starting up... 🚀
   2017/03/22 10:45:41 Looking for updates...
   2017/03/22 10:45:41 ⚙ Default configuration generated
   2017/03/22 10:45:41 ⌘ No bridge configuration found. Starting local discovery...
   2017/03/22 10:45:44 ⌘ Found bridge. Starting user registration.
   PLEASE PUSH THE BLUE BUTTON ON YOUR HUE BRIDGE...
   ```
4. Now you have to allow Kelvin to talk to your bridge by pushing the blue button on top of your physical Hue bridge. Kelvin will wait one minute for you to push the button. If you didn't make it in time just start it again with step 3.
5. Once you pushed the button you should see something like:
   ```
   2017/03/22 10:45:41 Kelvin v0.0.7 starting up... 🚀
   2017/03/22 10:45:41 Looking for updates...
   2017/03/22 10:45:41 ⚙ Default configuration generated
   2017/03/22 10:45:41 ⌘ No bridge configuration found. Starting local discovery...
   2017/03/22 10:45:44 ⌘ Found bridge. Starting user registration.
   PLEASE PUSH THE BLUE BUTTON ON YOUR HUE BRIDGE... Success!
   2017/03/22 10:45:59 💡 Devices found on current bridge:
   2017/03/22 10:45:59 | Name                 |  ID | Reachable | On    | Dimmable | Temperature | Color | Cur. Temp | Cur. Bri |
   2017/03/22 10:45:59 | Dining table         |   5 | false     | false | true     | true        | true  |     2638K |       92 |
   2017/03/22 10:45:59 | Poweroutlet          |   6 | true      | false | false    | false       | false |         - |        0 |
   2017/03/22 10:45:59 | Window               |   1 | false     | false | true     | true        | true  |     2197K |       72 |
   2017/03/22 10:45:59 | Kitchen              |   2 | false     | false | true     | true        | true  |     2012K |       60 |
   2017/03/22 10:45:59 | Couch                |   3 | false     | false | true     | true        | true  |     2012K |       59 |
   2017/03/22 10:45:59 | Desk                 |   4 | true      | false | true     | false       | true  |     6500K |        0 |
   2017/03/22 10:45:59 Device Poweroutlet doesn't support any functionality we use. Exlude it from unnessesary polling.
   2017/03/22 10:45:59 🌍 Location not configured. Detecting by IP
   2017/03/22 10:45:59 🌍 Detected location: Hamburg, Germany (53.5553, 9.995).
   2017/03/22 10:45:59 💡 Starting cyclic update for Desk
   2017/03/22 10:45:59 💡 Starting cyclic update for Window
   2017/03/22 10:45:59 💡 Starting cyclic update for Dining table
   2017/03/22 10:45:59 💡 Starting cyclic update for Kitchen
   2017/03/22 10:45:59 💡 Starting cyclic update for Couch
   2017/03/22 10:45:59 Configuring intervals for Wednesday March 22 2017
   2017/03/22 10:45:59 Managing lights for interval 06:21 to 18:40
   ```
6. Wohoo! Kelvin is up and running! Well done!
7. Kelvin is now managing your lights and will gradually adjust the color temperature and brightness for you. Give it a try by switching lights on and off to see how Kelvin reacts. If you want to adjust the default schedule to your needs, just read on and edit the configuration.

# Configuration
Kelvin will create it's configuration file `config.json` in the current directory and store all necessary information to operate in it. By default it is fully usable and looks like this:

```
{
  "bridge": {
    "ip": "192.168.10.37",
    "username": "lbCDGagZZ7JEYQX5iGxrjMIx2jIROgpXfsSjHmCv"
  },
  "location": {
    "latitude": 53.5553,
    "longitude": 9.995
  },
  "defaultColorTemperature": 2750,
  "defaultBrightness": 100,
  "beforeSunrise": [
    {
      "time": "4:00AM",
      "colorTemperature": 2000,
      "brightness": 60
    }
  ],
  "afterSunset": [
    {
      "time": "8:00PM",
      "colorTemperature": 2300,
      "brightness": 80
    },
    {
      "time": "10:00PM",
      "colorTemperature": 2000,
      "brightness": 60
    }
  ],
  "ignoredDeviceIDs": []
}
```
As the configuration file is a simple text file in JSON format you can display and edit it with you favorite text editor. Just make sure you keep the JSON structure valid. If something goes wrong fix it using [JSONLint](http://jsonlint.com/) or just delete the `config.json` and let Kelvin generate a configuration from scratch.

The configuration contains the following fields:

| Name | Description |
| ---- | ----------- |
| bridge | This element contains the IP and username of your Philips Hue bridge. Both values are usually obtained automatically. If the lookup fails you can fill in this details by hand. |
| location | This element contains the latitude and longitude of your location on earth. Both values are determined by your public IP. If this fails, is inaccurate or you want to change it manually just fill in your own coordinates. |
| defaultColorTemperature | This default color temperature will be used between sunrise and sunset. Valid values are between 2000K and 6500K. See [Wikipedia](https://en.wikipedia.org/wiki/Color_temperature) for reference values. |
| defaultBrightness | This default brightness value will be used between sunrise and sunset. Valid values are between 0% and 100%. |
| beforeSunrise | This element contains a list of timestamps and their configuration you want to set between midnight and sunrise of any given day. The *time* value must follow the `XX:XXAM/PM` format. *colorTemperature* and *brightness* must follow the same rules as the default values. |
| afterSunset | This element contains a list of timestamps and their configuration you want to set between sunset and midnight of any given day. The *time* value must follow the `XX:XXAM/PM` format. *colorTemperature* and *brightness* must follow the same rules as the default values. |
| ignoredDeviceIDs | This element contains a list of device IDs which will be exluded from Kelvin's automatic adjustments. Kelvin will print all device IDs found on your bridge during startup. Just add them (seperated by comma) to this list if you want to exlude them.|

After altering the configuration you have to restart Kelvin. Just kill the running instance (`Ctrl+C` or `kill $PID`) or send a HUP signal (`kill -s HUP $PID`) to the process to restart (unix only).

# Raspberry Pi
A [Raspberry Pi](https://www.raspberrypi.org/) is the **perfect** device to run Kelvin on. It's cheap, it's small and it consumes very little energy. Recently the [Raspberry Pi Zero W](https://www.raspberrypi.org/products/pi-zero-w/) was released which makes your Kelvin hardware look like this (plus a power cord):

![Raspberry Pi Zero W](https://www.raspberrypi.org/wp-content/uploads/2017/02/zero-wireless.png)

But any other model of the Raspberry Pi will be sufficient. To setup Kelvin on a Raspberry Pi follow the installation guide [here](https://www.raspberrypi.org/documentation/installation/). Once your Pi is up and running (booting, connected to your network and the internet) just download the latest `linux-arm` release and follow the steps in [Installation](#installation).

# Troubleshooting
If anything goes wrong keep calm and follow these steps:

1. Make sure the Philips Hue bridge is configured and working in your network. Kelvin will need it to communicate with your lights. If you got the hue app running on your smartphone you should be fine. Otherwise follow the Philips Hue manual to configure your lights.

2. To identify the IP address of your bridge open [this](https://www.meethue.com/api/nupnp) link in your browser. After you got the IP address enter `http://<bridge IP address>/debug/clip.html` into your browser. You should see the debug page of you hue bridge. If this fails please follow the Philips Hue manual to configure your bridge.

3. Make sure the Philips Hue bridge is reachable from the computer Kelvin will run on. Enter the command `ping <bridge IP address>` in a terminal window or on a remote console. You should see packages reaching the destination IP address. If this fails you might have a network issue.

4. Make sure you downloaded the latest release for your operating system and CPU architecture. If you are not sure stick to the most appropriate `amd64` release or `arm` if you are using a Raspberry Pi.

5. If all this doesn't help, feel free to open an [issue](https://github.com/stefanwichmann/kelvin/issues) on github.

# Development & Participation
If you want to tinker with Kelvin and it's inner workings, feel free to do so. To get started you can simple clone the main repository in your `GOPATH` by executing the following commands:
```
cd $GOPATH
go get -v github.com/stefanwichmann/kelvin
```
Make sure you have set up your [go](https://www.golang.org) development environment by following the steps in the official [documentation](https://golang.org/doc/).

If you have ideas how to improve Kelvin I will gladly accept pull requests from your forks or discuss them with you through an [issue](https://github.com/stefanwichmann/kelvin/issues).
