This repository contains the design files for the PCB found on Kauil. The main motivation behind the desigh of this PCB is to have a platform for testing and implementing new hardware easily. Also it should be easy for new students joining the team to start developing for it, this is why we choosed to use the same development board as a core as the one used in the microcontroller (embedded systems) class, the [STM32F3 discovery](http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/PF254044).

The design files were made using [DipTrace](http://diptrace.com/) which can be downloaded freely for 30 days, or without time limitations but with a 300 pin limitation  (this design contains more than 300 pins).

## Content of the repository

+ **BOM:** This folder contains some spreadsheets with components used for the PCB, costs and suplier links.
+ **Gerbers:** This folder contains the files that were sent to the manufacturer to produce the actual PCB, they were exported from DipTrace.
+ **Lib:** This folder contains library files for DipTrace with all schematic components and PCB patterns used on the PCB, we do not use any external library besides the one provided by DipTrace and the ones found here, so the design is portable.
+ **PCB.dip:** A DipTrace file with the PCB layout and routing.
+ **SchematicDiptrace.dch:** A DipTrace file with the schematic for the PCB, it contains information about logical connections of the board.

In addition to these files, some other pdfs show the design without requiring an installation of DipTrace.

## Known issues:

V2 of the PCB is the one that was manufactured and is found on Kauil, while doing the code implementation of it we've found several problems with the design that had to be fixed for the system to work, the following list explains them, and the fix that was applied, future versions of the board should have them fixed.

* The SDA pin from I2C2 (Used for the MD03 boards) was previously located
  on pin PF0, sadly this pin is occupied by an 8MHz clock signal that comes
  from the STLink microcontroller on the STM32F3 discovery (U2) and can't
  be changed without slowing down the main microcontroller, the actual
  PF0 pin on this board is not connected to the microcontroller, and has to
  be enabled by the SB12 jumper. To fix this the PCB was modified to 
  connect what came into PF0 before to PA10, this pin was the Rx pin for
  USART1 before, but if required it can be assigned to PC5 which is used
  for the STLink but suldn't be much trouble while running code (because
  STLink is just used for programming and debugging), it's important to
  note that this last jumper wire hasn't been soldered yet.
  (Peripherals affected: I2C2 (MD03), USART1 (Rx pin) )

* The A and B channels for both encoders were assigned incorrectly to pins
  on the STM32F3, these were all being already used for interrupts on both
  sensor chips, the LSM303DLHC and the L3GD20, to leave these pins free 
  for future use we chose to move the encoders to other pins, changes made
  were:

      PE4 -> PF9
      PE5 -> PF10
      PE0 -> PB8
      PE1 -> PB9

  (Periherals affected: Encoders and external interrupts for sensors 
   LSM303DLHC and the L3GD20)

* I2C1 has 2 connectors for 2 aditional devices beside the magnetometer
  that's internally connected to it on the discovery board, but the 
  connectors provided do not have a any voltage on pins labeled as 3.3V
  all other pins are correctly connected and pull ups are already fitted
  internally on the discovery board, but to get tha 3.3 voltage a jumper
  wire has to be soldered to either STM32's 3.3V output (recommended to 
  prevent having different voltages on the power line and the communication
  lines when pulled up, because pull ups are already connected to the 3.3V
  outputted by the STM32) or the output of the 3.3V regulator of the PCB. 
  (Peripherals affected: I2C1 external connectors)

