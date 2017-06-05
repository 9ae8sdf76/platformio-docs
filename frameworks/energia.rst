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

.. _framework_energia:

Framework ``energia``
=====================
Energia Wiring-based framework enables pretty much anyone to start easily creating microcontroller-based projects and applications. Its easy-to-use libraries and functions provide developers of all experience levels to start blinking LEDs, buzzing buzzers and sensing sensors more quickly than ever before.

For more detailed information please visit `vendor site <http://energia.nu/reference/>`_.

.. contents::

Platforms
---------
.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - :ref:`platform_timsp430`
      - MSP430 microcontrollers (MCUs) from Texas Instruments (TI) are 16-bit, RISC-based, mixed-signal processors designed for ultra-low power. These MCUs offer the lowest power consumption and the perfect mix of integrated peripherals for thousands of applications.

    * - :ref:`platform_titiva`
      - Texas Instruments TM4C12x MCUs offer the industrys most popular ARM Cortex-M4 core with scalable memory and package options, unparalleled connectivity peripherals, advanced application functions, industry-leading analog integration, and extensive software solutions.

Boards
------

.. note::
    * You can list pre-configured boards by :ref:`cmd_boards` command or
      `PlatformIO Boards Explorer <http://platformio.org/boards>`_
    * For more detailed ``board`` information please scroll tables below by horizontal.

TI
~~

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

    * - ``lplm4f120h5qr``
      - `TI LaunchPad (Stellaris) w/ lm4f120 (80MHz) <http://www.ti.com/tool/ek-lm4f120xl>`_
      - :ref:`TI TIVA <platform_titiva>`
      - :ref:`Yes <debugging>`
      - LPLM4F120H5QR
      - 80 MHz
      - 256 Kb
      - 32 Kb

    * - ``lpmsp430f5529``
      - `TI LaunchPad MSP-EXP430F5529LP <http://www.ti.com/ww/en/launchpad/launchpads-msp430-msp-exp430f5529lp.html>`_
      - :ref:`TI MSP430 <platform_timsp430>`
      - :ref:`Yes <debugging>`
      - MSP430F5529
      - 16 MHz
      - 128 Kb
      - 8 Kb

    * - ``lpmsp430fr4133``
      - `TI LaunchPad MSP-EXP430FR4133LP <http://www.ti.com/tool/msp-exp430fr4133>`_
      - :ref:`TI MSP430 <platform_timsp430>`
      - :ref:`Yes <debugging>`
      - MSP430FR4133
      - 8 MHz
      - 16 Kb
      - 2 Kb

    * - ``lpmsp430fr5739``
      - `TI FraunchPad MSP-EXP430FR5739LP <http://www.ti.com/tool/msp-exp430fr5739>`_
      - :ref:`TI MSP430 <platform_timsp430>`
      - :ref:`Yes <debugging>`
      - MSP430FR5739
      - 16 MHz
      - 16 Kb
      - 0.5 Kb

    * - ``lpmsp430fr5969``
      - `TI LaunchPad MSP-EXP430FR5969LP <http://www.ti.com/ww/en/launchpad/launchpads-msp430-msp-exp430fr5969.html>`_
      - :ref:`TI MSP430 <platform_timsp430>`
      - :ref:`Yes <debugging>`
      - MSP430FR5969
      - 8 MHz
      - 64 Kb
      - 2 Kb

    * - ``lpmsp430fr6989``
      - `TI LaunchPad MSP-EXP430FR6989LP <http://www.ti.com/tool/msp-exp430fr6989>`_
      - :ref:`TI MSP430 <platform_timsp430>`
      - :ref:`Yes <debugging>`
      - MSP430FR6989
      - 8 MHz
      - 128 Kb
      - 2 Kb

    * - ``lpmsp430g2553``
      - `TI LaunchPad MSP-EXP430G2553LP <http://www.ti.com/ww/en/launchpad/launchpads-msp430-msp-exp430g2.html>`_
      - :ref:`TI MSP430 <platform_timsp430>`
      - :ref:`Yes <debugging>`
      - MSP430G2553
      - 16 MHz
      - 16 Kb
      - 0.5 Kb

    * - ``lptm4c1230c3pm``
      - `TI LaunchPad (Tiva C) w/ tm4c123 (80MHz) <http://www.ti.com/ww/en/launchpad/launchpads-connected-ek-tm4c123gxl.html>`_
      - :ref:`TI TIVA <platform_titiva>`
      - :ref:`Yes <debugging>`
      - LPTM4C1230C3PM
      - 80 MHz
      - 256 Kb
      - 32 Kb

    * - ``lptm4c1294ncpdt``
      - `TI LaunchPad (Tiva C) w/ tm4c129 (120MHz) <http://www.ti.com/ww/en/launchpad/launchpads-connected-ek-tm4c1294xl.html>`_
      - :ref:`TI TIVA <platform_titiva>`
      - :ref:`Yes <debugging>`
      - LPTM4C1294NCPDT
      - 120 MHz
      - 1024 Kb
      - 256 Kb
