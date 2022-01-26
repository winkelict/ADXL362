Winkel ICT ADXL362
==================
Library for ADXL362 accelerometer: unique because of it's 
- ultralow (lowest of +/- 120 reviewed datasheets) power (0,270ua) usage
- autonomous motion switch functionality. 

Thoroughly tested low memory footprint library, complete implementation of datasheet / functionality.
Focus on ease of use and easy debugging (every error has a unique negative status code returned by most functions)

## Hardware
- SparkFun Triple Axis Accelerometer Breakout - ADXL362 (SEN-11446) https://www.sparkfun.com/products/11446
- SparkFun Wake on Shake (SEN-11447) - https://www.sparkfun.com/products/11447
- Breakout boards also available available on Ebay at a much lower price! 

## Quick start
Important deviations from datasheet terminology:
- link/loop is called sequential (see configureSequential)
- wakeup mode is implemented as just a bandwith / odr mode: 

	ad_bandwidth_hz_6_wakeup_ultralowpower

For most use-cases the 4 main functions cover all functionality:
- activateMeasure
- activateAutonomousMotionSwitch
- activateFreeFallDetection
- activateCustomDetection

In combination with:

For measuring:
- getXYZ
- getTemperature

Or for working with interrupts:
- isActInterrupt
- isInactInterrupt
- isAwake

### Debugging settings (ADXL362.h)
Uncomment to disable debugging, default=debug enabled

	#define ADXL362_DEBUG

In debug mode extra checks are done for parameter combinations, in rare cases (when no status code can be returned) a serial monitor print.

Also the printregisters function is disabled, see:

To use the printregisters function for debugging (prints all register values by hex address + name), will need to install https://playground.arduino.cc/Code/HashMap/

And uncomment:

	//#define ADXL362_HASHMAPLIBINSTALLED

This setting will cause all written registry values to be read back and checked if correctly written. Will slow down performance / energy usage so might not be suitable for production.

	#define ADXL362_VERIFY_REG_READBACK

### Measuring XYZ/Temp

### Motion Switch

### Freefall detection

### Custom detection

### Misc settings (ADXL362.h)
Enable build in hamming error detection

	//#define ADXL362_VERIFY_REG_WRITES_HAMMING

Skip writing the most significant byte of all MSB registers (_H registers) (UNTESTED)

	//ADXL362_POWERSAVE_SKIPWRITE_MSB

At request can make an SPI stub available which will simulate the accelerometer (not included).

	//#define ADXL362_USE_SPI_STUB

My measurements identified some corrections to the wakeupmode ODR and the time treshholds, these might have to be adjusted for your specific chip.

	#define ADXL362_WAKEUPMODE_ACTUALODR 4.20778f

	#define ADXL362_TIMECORRECTION_INPERCENT -10

## Goals / Philosophy
- Functions / parameters should not allow options/combinations that the accelerometer cannot execute
- Inuitive usage of all code, functions, parameters
  - when not in conflict with first rule, use datasheet terminology
- All functions return a unique negative status (if possible) value when an error occured, and exit the function immediately after that
  - this makes it easy to look for which problem occured and fix it
- Development: start with debug mode 'on', allowing for extra checks of correct parameter combinations.
- as little RAM usage as possible
- as little flash usage as possible

## TODO's
- test FIFO functionality
- test external clock / sample trigger parameters of activity and inactivity functions
- implement self-test
- test setting
	ADXL362_POWERSAVE_SKIPWRITE_MSB