### Description
This project aims to exploring both the STM32F446RE Microcontroller and the ARM Cortex-M4 architecture in a "low level" perspective.

Reference:  [eedesignlab.com](https://eedesignlab.com/exploring-the-stm32f446-mcu-introduction/)



### Contents

  * **Development Tools**
    * Hardware
    * Software

  * **Installation**
    * Utilities
    * Project
  
  * **Build**

  * **Flash**

  
### Development Tools

##### Hardware

- [NUCLEO-F446RE](https://www.st.com/en/evaluation-tools/nucleo-f446re.html).
- [SEGGER J-Link EDU](https://www.segger.com/products/debug-probes/j-link/models/j-link-edu/)


##### Software

- Toolchain: [GNU Arm Embedded](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads).


### Installation

##### Utilities

- Execute `sudo apt-get install build-essential` for installing the *make* utility.

##### Project

- Clone this project.
- Download and extract the [GNU Arm Embedded](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) into the toolchain folder.

### Build
 
 Inside the *target-stm32f446* folder:
 - Execute 'make help' for show all available targets.
 - Execute 'make' for build the project in debug mode.
 - Execute 'make REL=0' for build the project in release mode.

### Flash