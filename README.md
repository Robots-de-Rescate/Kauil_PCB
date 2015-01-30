This repository contains the design files for the PCB found on Kauil. The main motivation behind the desigh of this PCB is to have a platform for testing and implementing new hardware easily. Also it should be easy for new students joining the team to start developing for it, this is why we choosed to use the same development board as a core as the one used in the microcontroller (embedded systems) class, the [STM32F3 discovery](http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/PF254044).

The design files were made using [DipTrace](http://diptrace.com/) which can be downloaded freely for 30 days, or without time limitations but with a 300 pin limitation (this design contains more than 300 pins).

## Content of the repository

+ **BOM:** This folder contains some spreadsheets with components used for the PCB, costs and suplier links.
+ **Gerbers:** This folder contains the files that were sent to the manufacturer to produce the actual PCB, they were exported from DipTrace.
+ **Lib:** This folder contains library files for DipTrace with all schematic components and PCB patterns used on the PCB, we do not use any external library besides the one provided by DipTrace and the ones found here, so the design is portable.
+ **PCB.dip:** A DipTrace file with the PCB layout and routing.
+ **PCB.jpg: ** A .jpg image of the PCB for quick reference without requiring DipTrace
+ **Schematic.dch:** A DipTrace file with the schematic for the PCB, it contains information about logical connections of the board.
+ **Schematic.jpg:** A .jpg image with the schematics used for the PCB, for easy acces for people that do not have DipTrace.

## Known issues:

V2 of the PCB is the one that was manufactured and is found on Kauil, while doing the code implementation of it we found several problems with the design that had to be corrected for the system to work, the following list explains them, and the fix that was applied, future versions of the board should have them permanently fixed.

* The SDA pin from I2C2 (Used for the MD03 boards) was previously located
  on pin PF0, but this pin is occupied by an 8MHz clock signal that comes
  from the STLink microcontroller on the STM32F3 discovery (U2) and can't
  be changed without slowing down the main microcontroller (the actual
  PF0 pin on the header of the Discovery board is not connected to the 
  microcontroller, and has to be enabled by the SB12 jumper). To fix this 
  the PCB was modified to connect what came into PF0 before to PA10, this 
  pin was the Rx pin for USART1 before, but if required it can be assigned 
  to PC5 which is used for the STLink but suldn't be much trouble while 
  running code (because STLink is just used for programming and debugging), 
  it's important to note that this last jumper wire hasn't been soldered yet.  
  (**Peripherals affected:** I2C2 (MD03), USART1 (Rx pin) )

* The A and B channels for both encoders were assigned incorrectly to pins
  on the STM32F3 that were being already used for interrupts on the two
  sensor chips the LSM303DLHC and the L3GD20, to leave these pins free 
  for future use we chose to move the encoders to other pins, changes made
  were:

      PE4 -> PF9
      PE5 -> PF10
      PE0 -> PB8
      PE1 -> PB9

  (**Periherals affected:** Encoders and external interrupts for sensors 
   LSM303DLHC and the L3GD20)

* The PCB has 4 SPI external connectors, the connector is supposed to 
  provide Ground and Voltage pins to easily connect any device to them,
  but sadly when the schematic was created the Ground pins were left
  unconnected to the actual ground trace. To fix this a wire has to be
  soldered on the bottom of the PCB so the ground is correctly connected.  
  (**Peripherals affected:** all SPI external connectors)

