..  Copyright (c) 2014-present PlatformIO <contact@platformio.org>
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
       http://www.apache.org/licenses/LICENSE-2.0
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

.. _platform_siliconlabsefm32:

Platform ``siliconlabsefm32``
=============================
Silicon Labs EFM32 Gecko 32-bit microcontroller (MCU) family includes devices that offer flash memory configurations up to 256 kB, 32 kB of RAM and CPU speeds up to 48 MHz. Based on the powerful ARM Cortex-M core, the Gecko family features innovative low energy techniques, short wake-up time from energy saving modes and a wide selection of peripherals, making it ideal for battery operated applications and other systems requiring high performance and low-energy consumption.

For more detailed information please visit `vendor site <http://www.silabs.com/products/mcu/32-bit/efm32-gecko/Pages/efm32-gecko.aspx>`_.

.. contents::

Packages
--------

.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - `framework-mbed <http://mbed.org>`__
      - mbed Framework

    * - `tool-openocd <http://openocd.org>`__
      - OpenOCD

    * - `toolchain-gccarmnoneeabi <https://launchpad.net/gcc-arm-embedded>`__
      - gcc-arm-embedded

.. warning::
    **Linux Users**:

    * Ubuntu/Debian users may need to add own "username" to the "dialout"
      group if they are not "root", doing this issuing a
      ``sudo usermod -a -G dialout yourusername``.
    * Install "udev" rules file `99-platformio-udev.rules <https://github.com/platformio/platformio-core/blob/develop/scripts/99-platformio-udev.rules>`_
      (an instruction is located in the file).
    * Raspberry Pi users, please read this article
      `Enable serial port on Raspberry Pi <https://hallard.me/enable-serial-port-on-raspberry-pi/>`__.


    **Windows Users:** Please check that you have correctly installed USB
    driver from board manufacturer



Frameworks
----------
.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - :ref:`framework_mbed`
      - The mbed framework The mbed SDK has been designed to provide enough hardware abstraction to be intuitive and concise, yet powerful enough to build complex projects. It is built on the low-level ARM CMSIS APIs, allowing you to code down to the metal if needed. In addition to RTOS, USB and Networking libraries, a cookbook of hundreds of reusable peripheral and module libraries have been built on top of the SDK by the mbed Developer Community.

Boards
------

.. note::
    * You can list pre-configured boards by :ref:`cmd_boards` command or
      `PlatformIO Boards Explorer <http://platformio.org/boards>`_
    * For more detailed ``board`` information please scroll tables below by
      horizontal.

Silicon Labs
~~~~~~~~~~~~

.. list-table::
    :header-rows:  1

    * - ID
      - Name
      - Platform
      - Debug
      - Microcontroller
      - Frequency
      - Flash
      - RAM

    * - ``efm32gg_stk3700``
      - `Silicon Labs EFM32GG-STK3700 (Giant Gecko) <https://developer.mbed.org/platforms/EFM32-Giant-Gecko/>`_
      - :ref:`Silicon Labs EFM32 <platform_siliconlabsefm32>`
      - :ref:`Yes <debugging>`
      - EFM32GG990F1024
      - 48 MHz
      - 1024 Kb
      - 128 Kb

    * - ``efm32hg_stk3400``
      - `Silicon Labs SLSTK3400A USB-enabled (Happy Gecko) <https://developer.mbed.org/platforms/EFM32-Happy-Gecko/>`_
      - :ref:`Silicon Labs EFM32 <platform_siliconlabsefm32>`
      - :ref:`Yes <debugging>`
      - EFM32HG322F64
      - 24 MHz
      - 64 Kb
      - 8 Kb

    * - ``efm32lg_stk3600``
      - `Silicon Labs EFM32LG-STK3600 (Leopard Gecko) <https://developer.mbed.org/platforms/EFM32-Leopard-Gecko/>`_
      - :ref:`Silicon Labs EFM32 <platform_siliconlabsefm32>`
      - :ref:`Yes <debugging>`
      - EFM32LG990F256
      - 48 MHz
      - 256 Kb
      - 32 Kb

    * - ``efm32pg_stk3401``
      - `Silicon Labs SLSTK3401A (Pearl Gecko) <https://developer.mbed.org/platforms/EFM32-Pearl-Gecko/>`_
      - :ref:`Silicon Labs EFM32 <platform_siliconlabsefm32>`
      - :ref:`Yes <debugging>`
      - EFM32PG1B200F256
      - 40 MHz
      - 256 Kb
      - 32 Kb

    * - ``efm32wg_stk3800``
      - `Silicon Labs EFM32WG-STK3800 (Wonder Gecko) <https://developer.mbed.org/platforms/EFM32-Wonder-Gecko/>`_
      - :ref:`Silicon Labs EFM32 <platform_siliconlabsefm32>`
      - :ref:`Yes <debugging>`
      - EFM32WG990F256
      - 48 MHz
      - 256 Kb
      - 32 Kb

    * - ``efm32zg_stk3200``
      - `Silicon Labs EFM32ZG-STK3200 (Zero Gecko) <https://developer.mbed.org/platforms/EFM32-Zero-Gecko/>`_
      - :ref:`Silicon Labs EFM32 <platform_siliconlabsefm32>`
      - :ref:`Yes <debugging>`
      - EFM2ZG222F32
      - 24 MHz
      - 32 Kb
      - 4 Kb
