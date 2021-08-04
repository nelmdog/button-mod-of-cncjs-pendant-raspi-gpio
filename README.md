# Rework of the cncjs-pendant-raspi-gpio Code Allowing for Multiple Physical Buttons

I'm hoping to create a set of user triggerable physical buttons for CNCJS based on the code for the cncjs-pendant-raspi-gpio. For my own Marlin and SKR Pro 1.2 based MPCNC setup I want to be able to trigger the tool on and off, and initiate a feedhold from physical buttons mounted next to the machine.

My coding-fu is weak so I'm hoping to use this existing code as a basis for something that works, even if I produce a very inellegant solution to my problem!

You will require two resistors per button, a 1k and a 10k to build as a current limiter/voltage divider to protect the GPIO.

#### Settings:

Pin Number, GPIO Pin, Button Function, GCode Called

Pin 16, GPIO 23, Stop Tool and Shutdown if held (Red button), M05

Pin 17, NA, 3.3v supply to bring GPIO high on button press, NA

Pin 18, GPIO24, Start Tool (Green Button), M03

Pin 20, NA, Common ground for all buttons, NA

Pin 22, GPIO25, Feed Hold function that efectively pauses the cut (Yellow button), M108 but requires 'emergency parser' to be enabled in firmware to really be useful

![image-a](https://github.com/nelmdog/button-mod-of-cncjs-pendant-raspi-gpio/raw/master/docs/image-a.jpg)

If this works, hopefully it will be of help to others, or a basis for a more elegent and effecient method!

All constructive comments welcomed.

Thanks!

Installation (borrowed from original readme...)

## Installation

#### Manual Install
```
# Clone Repository
cd ~/
wget https://github.com/nelmdog/button-mod-of-cncjs-pendant-raspi-gpio/archive/master.zip
unzip master.zip
git clone https://github.com/nelmdog/button-mod-of-cncjs-pendant-raspi-gpio.git
cd button-mod-of-cncjs-pendant-raspi-gpio-master
npm install
chmod u+x bin/button-mod-of-cncjs-pendant-raspi-gpio
```

## Usage
Run `bin/button-mod-of-cncjs-pendant-raspi-gpio` to start.

## Original readme follows:

.

.

.

.

# Simple Raspberry Pi GPIO Pendant control for CNCjs.

[![NPM](https://nodei.co/npm/cncjs-pendant-raspi-gpio.png?compact=true)](https://nodei.co/npm/cncjs-pendant-raspi-gpio/)

![image-1](https://github.com/cncjs/cncjs-pendant-raspi-gpio/raw/master/docs/image-1.jpg)

## Installation
#### NPM Install (local)
```
npm install cncjs-pendant-raspi-gpio
```
#### NPM Install (global) [Recommended]
```
sudo npm install -g cncjs-pendant-raspi-gpio@latest --unsafe-perm --build-from-source
```

#### Manual Install
```
# Clone Repository
cd ~/
#wget https://github.com/cncjs/cncjs-pendant-raspi-gpio/archive/master.zip
#unzip master.zip
git clone https://github.com/cncjs/cncjs-pendant-raspi-gpio.git
cd cncjs-pendant-raspi-gpio*
npm install
```

## Usage
Run `bin/cncjs-pendant-raspi-gpio` to start. Pass --help to `cncjs-pendant-raspi-gpio` for more options.

Eamples:

```
bin/cncjs-pendant-keyboard --help
node bin/cncjs-pendant-raspi-gpio  --port /dev/ttyUSB0
```

#### Auto Start

###### Install [Production Process Manager [PM2]](http://pm2.io)
```
# Install PM2
sudo npm install -g pm2

# Setup PM2 Startup Script
# sudo pm2 startup  # To Start PM2 as root
pm2 startup  # To start PM2 as pi / current user
  #[PM2] You have to run this command as root. Execute the following command:
  sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u pi --hp /home/pi

# Start CNCjs (on port 8000, /w Tinyweb mount point) with PM2
## pm2 start ~/.cncjs/cncjs-pendant-raspi-gpio/bin/cncjs-pendant-raspi-gpio -- --port /dev/ttyUSB0
pm2 start $(which cncjs-pendant-raspi-gpio) -- --port /dev/ttyUSB0

# Set current running apps to startup
pm2 save

# Get list of PM2 processes
pm2 list
```

#### Button Presses
 1. G-Code: M9
 2. G-Code: M8
 3. G-Code: M7
 4. G-Code: $X "Unlock"
 5. G-Code: $X "Unlock"
 6. G-Code: $SLP "Sleep"
 7. G-Code: $SLP "Sleep"
 8. G-Code: $H "Home"

#### Press & Hold
 - 3 Sec: sudo poweroff "Shutdown"

## Wiring 

See the [fivdi/onoff](https://www.npmjs.com/package/onoff) Raspberry Pi GPIO NodeJS repository for more infomation.
![nodejs onoff diagram](https://raw.githubusercontent.com/fivdi/onoff/master/examples/light-switch.png)

![raspberry_pi_circuit_note](http://www.jameco.com/Jameco/workshop/circuitnotes/raspberry_pi_circuit_note_fig2a.jpg)

![image-4](https://github.com/cncjs/cncjs-pendant-raspi-gpio/raw/master/docs/image-4.jpg)

![image-3](https://github.com/cncjs/cncjs-pendant-raspi-gpio/raw/master/docs/image-3.jpg)

![image-2](https://github.com/cncjs/cncjs-pendant-raspi-gpio/raw/master/docs/image-2.jpg)
