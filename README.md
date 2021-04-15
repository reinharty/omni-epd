# VSMP-EPD Abstraction
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)


EPD (electronic paper display) class abstractions for very slow media players

There are several implementations of very slow media players (VSMP), many in Python. The problem with a lot of these is that the code is often for one specific type of display, or perhaps a family of displays. This project abstracts the EPD classes into a common interface so a variety of displays can be interchangeably used in the same project.

For VSMP project maintainers this expands the number of displays you can use for your project without having to code around each one. To utilize this in your VSMP project read the usage instructions. For a list of (known) projects that use this abstraction see the list below.

## Install

```

sudo pip3 install vsmp-epd

```

## Usage

Usage in this case refers to VSMP project implementers that wish to abstract their EPD code with this library. In general, this is pretty simple. This library is meant to be very close to a 1:1 replacement for existing EPD code you may have in your project. Function names may vary slightly but most calls are very similar.


Below is an example utilizing the built in in `MockDisplay` object. This is will emulate the calls of a real EPD without the need for actual hardware.

```
from vsmp_epd import displayfactory

# the vsmp name of the display you want to load.
displayName = "vsmp_epd.mock"

# get a list of all supported displays from the display factory
allDisplays = displayfactory.list_supported_displays()

# check if this exists - not necessary but good practice
if( display in allDisplays ):
  epd = displayfactory.load_display_driver(displayName)

  # get the width and height
  print(f"Loaded {displayName} with width {epd.width} and height {epd.height}")

  # perform actions on the epd
  epd.prepare()

  epd.display(get_image())  # see below for more on this function

  epd.sleep()

else:
  print('Couldn't find that display')

```

### VirtualEPD Object

Objects returned by the `displayfactory` class all inherit methods from the `VirtualEPD` class. The following methods are available once the object is loaded. Be aware that not all displays may implement all methods but `display` is required.

* `width` and `height` - these are convience attributes to get the width and height of the display in your code. See the above example for their use.
* `prepare()` - does any initializing information on the display. This is waking up from sleep or doing anything else prior to a new image being drawn.
* `display(image)` - draws an image on the display. The image must be a [Pillow Image](https://pillow.readthedocs.io/en/stable/reference/Image.html) object.
* `sleep()` - puts the display into sleep mode, if available for that device. Generally this is lower power consumption and maintains better life of the display.
* `clear()` - clears the display
* `close()` - performs any cleanup operations and closes access to the display. Use at the end of a program or when the object is no longer needed.

## Displays Implemented
Below is a list of displays currently implemented in the library. The VSMP Device Name is what you'd pass to `displaymanager.load_display_driver(deviceName)` to load the correct device driver. Generally this is the `packagename.devicename` Devices in __bold__ have been tested on actual hardware while others have been implemented but not verified. This often happens when multiple displays use the same libraries but no physical verification has happened for all models.

| Device Library | Device Name | VSMP Device Name |
|:---------------|:------------|:-----------------|
| [Waveshare](https://github.com/waveshare/e-Paper) | [1.02inch E-Ink display module](https://www.waveshare.com/1.02inch-e-Paper-Module.htm) | waveshare_epd.epdlin02 |
|  | [1.54inch E-Ink display module](https://www.waveshare.com/1.54inch-e-Paper-Module.htm) | waveshare_epd.epdlin54 <br> waveshare_epd.epdlin54_V2 |
|  | [1.54inch e-Paper Module B](https://www.waveshare.com/1.54inch-e-Paper-Module-B.htm) | waveshare_epd.epdlin54b <br> waveshare_epd.epdlin54b_V2 |
|  | [1.54inch e-Paper Module C ](https://www.waveshare.com/1.54inch-e-Paper-Module-C.htm) | waveshare_epd.epdlin54c |
|  | [2.13inch e-Paper HAT](https://www.waveshare.com/2.13inch-e-Paper-HAT.htm) | waveshare_epd. epd2in13 <br>  waveshare_epd. epd2in13_V2 |
|  | [2.13inch e-Paper HAT B](https://www.waveshare.com/2.13inch-e-Paper-HAT-B.htm) | waveshare_epd. epd2in13b_V3 |
|  | [2.13inch e-Paper HAT C ](https://www.waveshare.com/2.13inch-e-Paper-HAT-C.htm) | waveshare_epd. epd2in13bc |
|  | [2.13inch e-Paper HAT D](https://www.waveshare.com/2.13inch-e-Paper-HAT-D.htm) | waveshare_epd. epd2in13d |
|  | [2.66inch e-Paper Module](https://www.waveshare.com/2.66inch-e-Paper-Module.htm) | waveshare_epd.epd2in66 |
|  | [2.66inch e-Paper Module B](https://www.waveshare.com/2.66inch-e-Paper-Module-B.htm) | waveshare_epd.epd2in66b |
|  | [2.7inch e-Paper HAT](https://www.waveshare.com/2.7inch-e-Paper-HAT.htm) | waveshare_epd.epd2in7 |
|  | [2.7inch e-Paper HAT B](https://www.waveshare.com/2.7inch-e-Paper-HAT-B.htm) | waveshare_epd.epd2in7b <br> waveshare_epd.epd2in7b_V2 |
|  | [2.9inch e-Paper Module](https://www.waveshare.com/2.9inch-e-Paper-Module.htm) | waveshare_epd.epd2in9 <br> waveshare_epd.epd2in9_V2 <br> waveshare_epd.epd2in9b_V3 |
|  | [2.9inch e-Paper Module B](https://www.waveshare.com/2.9inch-e-Paper-Module-B.htm) | waveshare_epd.epd2in9bc |
|  | [2.9inch e-Paper Module C](https://www.waveshare.com/2.9inch-e-Paper-Module-C.htm) | waveshare_epd.epd2in9bc |
|  | [2.9inch e-Paper HAT D](https://www.waveshare.com/2.9inch-e-Paper-HAT-D.htm) | waveshare_epd.epd2in9d |
|  | [3.7inch e-Paper HAT](https://www.waveshare.com/3.7inch-e-Paper-HAT.htm) | waveshare_epd.epd3in7 |
|  | [4.2inch e-Paper Module](https://www.waveshare.com/4.2inch-e-Paper-Module.htm) |waveshare_epd.epd4in2 <br> waveshare_epd.epd4in2b_V2 |
|  | [4.2inch e-Paper Module B](https://www.waveshare.com/4.2inch-e-Paper-Module-B.htm) |waveshare_epd.epd4in2bc |
|  | [4.2inch e-Paper Module C](https://www.waveshare.com/4.2inch-e-Paper-Module-C.htm) |waveshare_epd.epd4in2bc |
|  | [5.65inch e-Paper Module F](https://www.waveshare.com/5.65inch-e-Paper-Module-F.htm) |waveshare_epd.epd5in65f |
|  | [5.83inch e-Paper HAT](https://www.waveshare.com/5.83inch-e-Paper-HAT.htm) |waveshare_epd.epd5in83 <br> waveshare_epd.epd5in83_V2 |
|  | [5.83inch e-Paper HAT B](https://www.waveshare.com/5.83inch-e-Paper-HAT-B.htm) |waveshare_epd.epd5in83b_V2 |
|  | [5.83inch e-Paper HAT C](https://www.waveshare.com/5.83inch-e-Paper-HAT-C.htm) |waveshare_epd.epd5in83bc |
|  | [7.5inch e-Paper HAT](https://www.waveshare.com/7.5inch-e-Paper-HAT.htm) |waveshare_epd.epd7in5 <br> __waveshare_epd.epd7in5_V2__ |
|  | [7.5inch HD e-Paper HAT](https://www.waveshare.com/7.5inch-HD-e-Paper-HAT.htm) |waveshare_epd.epd7in5_HD |
|  | [7.5inch HD e-Paper HAT B](https://www.waveshare.com/7.5inch-HD-e-Paper-HAT-B.htm) |waveshare_epd.epd7in5b_HD |
|  | [7.5inch e-Paper HAT B](https://www.waveshare.com/7.5inch-HD-e-Paper-HAT-B.htm)| waveshare_epd.epd7in5b_V2 |
|  | [7.5inch e-Paper HAT C](https://www.waveshare.com/7.5inch-e-Paper-HAT-C.htm) | waveshare_epd.epd7in5bc |


### Waveshare

The Waveshare device library is not available via the Package Installer for Python (pip) and must be installed manually. Instructions for this are:

```

git clone https://github.com/waveshare/e-Paper
cd e-Paper/RaspberryPi_JetsonNano/python/
sudo python3 setup.py install

```

## Implementing Projects
Below is a list of known projects currently utilizing `vsmp-epd`. If you're interested in building a very small media player, check them out.

## License
[GPLv3](/LICENSE)
