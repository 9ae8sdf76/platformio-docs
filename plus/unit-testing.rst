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

.. |PIOUTE| replace:: **PIO Unit Testing Engine**
.. |PIOUTF| replace:: *PIO Unit Testing Framework*

.. _unit_testing:

PIO Unit Testing
================

.. versionadded:: 3.0 (`PlatformIO Plus <https://pioplus.com>`_)

`Unit Testing (wiki) <https://en.wikipedia.org/wiki/Unit_testing>`_
is a software testing method by which individual units of source code, sets
of one or more MCU program modules together with associated control data,
usage procedures, and operating procedures, are tested to determine whether
they are fit for use. Unit testing finds problems early in the development cycle.

.. tip::
  **QUESTIONS? Ask us!** Chat button is located in the bottom right corner

.. contents::

Demo
----

Demo of `Local & Embedded: Calculator <https://github.com/platformio/platformio-examples/tree/develop/unit-testing/calculator>`_.

.. image:: ../_static/pioplus-unit-testing-demo.png
    :target: https://youtu.be/bo3VVRZVKhA

Test Types
----------

Desktop
~~~~~~~

PlatformIO wraps a test and a main program (from :ref:`projectconf_pio_src_dir`)
with |PIOUTF| and builds the final program using :ref:`platform_native`
development platform. Finally, PlatformIO runs tests on the host machine
(desktop or :ref:`ci` VM instance).

.. note::
    PlatformIO does not install any toolchains automatically for
    :ref:`platform_native` and requires ``GCC`` toolchain to be installed
    on your host machine.
    Please open Terminal and check that the ``gcc`` command is installed.

Embedded
~~~~~~~~

PlatformIO wraps a test and main firmware (from :ref:`projectconf_pio_src_dir`)
with |PIOUTF| and builds special firmware for a target device and deploy it.
Then, PlatformIO connects to the embedded device (board) using configured
Serial :ref:`projectconf_test_port` and communicate via
:ref:`projectconf_test_transport`. Finally, it runs tests on embedded side,
collects results, analyzes them and provides a summary on host machine side
(desktop).

.. note::

    Please note that the |PIOUTF| uses the first available ``Serial/UART``
    implementation (depending on a :ref:`projectconf_env_framework`) as a
    communication interface between the |PIOUTE| and target device. If you use
    ``Serial`` in your project, please wrap/hide Serial-based blocks with
    ``#ifndef UNIT_TEST`` macro.

    Also, you can create custom :ref:`projectconf_test_transport` and implement
    base interface.

Test Runner
-----------

Test Runner allows you to process specific environments or ignore tests using
"Glob patterns". You can also ignore tests for specific environments using a
:ref:`projectconf_test_ignore` option from :ref:`projectconf`.

Local
~~~~~
Allows you to run tests on a host machine or on target devices (boards) that are
directly connected to the host machine. In this case, need to use the
:ref:`cmd_test` command.

Remote
~~~~~~

Allows you to run tests on a remote machine or remote target devices (boards)
without having to depend on OS software, extra software, SSH, VPN or opening
network ports. Remote Unit Testing works in pair with :ref:`pio_remote`. In this
case, you need to use the special command :ref:`cmd_remote_test`.

PlatformIO supports multiple :ref:`ci` systems where you can run unit tests
on each integration stage. See real
`PlatformIO Remote Unit Testing Example <https://github.com/platformio/platformio-remote-unit-testing-example>`__.

.. _unit_testing_design:

Design
------

The |PIOUTE| design is based on a few isolated components:

1. **Main Program**. Contains the independent modules, procedures, functions or
   methods that will be the target candidates (TC) for testing.
2. **Unit Test**. A small independent program that is intended to re-use TC from
   the main program and apply tests to them.
3. **Test Processor**. The set of approaches and tools that will be used
   to apply tests for the environments from :ref:`projectconf`.

Workflow
--------

1. Create PlatformIO project using the :ref:`cmd_init` command. For Desktop Unit
   Testing (on the host machine), you need to use :ref:`platform_native`.

   .. code-block:: ini

        ; PlatformIO Project Configuration File
        ;
        ;   Build options: build flags, source filter, extra scripting
        ;   Upload options: custom port, speed and extra flags
        ;   Library options: dependencies, extra library storages
        ;
        ; Please visit documentation for the other options and examples
        ; http://docs.platformio.org/en/stable/projectconf.html

        ;
        ; Embedded platforms
        ;

        [env:uno]
        platform = atmelavr
        framework = arduino
        board = uno

        [env:nodemcu]
        platform = espressif8266
        framework = arduino
        board = nodemcuv2

        ;
        ; Desktop platforms (Win, Mac, Linux, Raspberry Pi, etc)
        ; See http://platformio.org/platforms/native
        ;

        [env:native]
        platform = native

2. Place source code for the main program in the ``src`` directory.
3. Wrap ``main()`` or ``setup()/loop()`` methods in the main program with a
   ``UNIT_TEST`` guard:

   .. code-block:: c

        /**
        * Arduino Wiring-based Framework
        */
        #ifndef UNIT_TEST
        #include <Arduino.h>
        void setup () {
          // some code...
        }

        void loop () {
          // some code...
        }
        #endif


   .. code-block:: c

        /**
        * Generic C/C++
        */
        #ifndef UNIT_TEST
        int main(int argc, char **argv) {
          // setup code...

          while (1) {
              // loop code...
          }
          return 0
        }
        #endif

4. Create a ``test`` directory in the root of your project. See :ref:`projectconf_pio_test_dir`.
5. Write the test using :ref:`unit_testing_api`. Each test is a small independent
   program with its own ``main()`` or ``setup()/loop()`` methods. Tests should
   start with ``UNITY_BEGIN()`` and finish with ``UNITY_END()``.

   .. warning::
     If your board does not support software reset via ``Serial.DTR/RTS``,
     you should add >2 seconds delay before ``UNITY_BEGIN()`.
     That time is needed to establish a ``Serial`` communication between the host
     machine and target device.

     .. code-block:: c

         delay(2000); // for Arduino framework
         wait(2);     // for ARM mbed framework
         UNITY_BEGIN();


6. Place the test in the ``test`` directory. If you have more than one test,
   split them into sub-folders. For example, ``test/test_1/*.[c,cpp,h]``,
   ``test_N/*.[c,cpp,h]``, etc. If there is no such directory in the ``test``folder,
   then |PIOUTE| will treat the source code of ``test`` folder as SINGLE test.
7. Run tests using the :ref:`cmd_test` command.

.. _unit_testing_api:

API
---

Summary of the `Unity Test API <https://github.com/ThrowTheSwitch/Unity#unity-test-api>`_:

* `Running Tests <https://github.com/ThrowTheSwitch/Unity#running-tests>`_

  - ``RUN_TEST(func, linenum)``

* `Ignoring Tests <https://github.com/ThrowTheSwitch/Unity#ignoring-tests>`_

  - ``TEST_IGNORE()``
  - ``TEST_IGNORE_MESSAGE (message)``

* `Aborting Tests <https://github.com/ThrowTheSwitch/Unity#aborting-tests>`_

  - ``TEST_PROTECT()``
  - ``TEST_ABORT()``

* `Basic Validity Tests <https://github.com/ThrowTheSwitch/Unity#basic-validity-tests>`_

  - ``TEST_ASSERT_TRUE(condition)``
  - ``TEST_ASSERT_FALSE(condition)``
  - ``TEST_ASSERT(condition)``
  - ``TEST_ASSERT_UNLESS(condition)``
  - ``TEST_FAIL()``
  - ``TEST_FAIL_MESSAGE(message)``

* `Numerical Assertions: Integers <https://github.com/ThrowTheSwitch/Unity#numerical-assertions-integers>`_

  - ``TEST_ASSERT_EQUAL_INT(expected, actual)``
  - ``TEST_ASSERT_EQUAL_INT8(expected, actual)``
  - ``TEST_ASSERT_EQUAL_INT16(expected, actual)``
  - ``TEST_ASSERT_EQUAL_INT32(expected, actual)``
  - ``TEST_ASSERT_EQUAL_INT64(expected, actual)``

  - ``TEST_ASSERT_EQUAL_UINT(expected, actual)``
  - ``TEST_ASSERT_EQUAL_UINT8(expected, actual)``
  - ``TEST_ASSERT_EQUAL_UINT16(expected, actual)``
  - ``TEST_ASSERT_EQUAL_UINT32(expected, actual)``
  - ``TEST_ASSERT_EQUAL_UINT64(expected, actual)``

  - ``TEST_ASSERT_EQUAL_HEX(expected, actual)``
  - ``TEST_ASSERT_EQUAL_HEX8(expected, actual)``
  - ``TEST_ASSERT_EQUAL_HEX16(expected, actual)``
  - ``TEST_ASSERT_EQUAL_HEX32(expected, actual)``
  - ``TEST_ASSERT_EQUAL_HEX64(expected, actual)``
  - ``TEST_ASSERT_EQUAL_HEX8_ARRAY(expected, actual, elements)``

  - ``TEST_ASSERT_EQUAL(expected, actual)``
  - ``TEST_ASSERT_INT_WITHIN(delta, expected, actual)``

* `Numerical Assertions: Bitwise <https://github.com/ThrowTheSwitch/Unity#numerical-assertions-bitwise>`_

  - ``TEST_ASSERT_BITS(mask, expected, actual)``
  - ``TEST_ASSERT_BITS_HIGH(mask, actual)``
  - ``TEST_ASSERT_BITS_LOW(mask, actual)``
  - ``TEST_ASSERT_BIT_HIGH(mask, actual)``
  - ``TEST_ASSERT_BIT_LOW(mask, actual)``

* `Numerical Assertions: Floats <https://github.com/ThrowTheSwitch/Unity#numerical-assertions-floats>`_

  - ``TEST_ASSERT_FLOAT_WITHIN(delta, expected, actual)``
  - ``TEST_ASSERT_EQUAL_FLOAT(expected, actual)``
  - ``TEST_ASSERT_EQUAL_DOUBLE(expected, actual)``

* `String Assertions <https://github.com/ThrowTheSwitch/Unity#string-assertions>`_

  - ``TEST_ASSERT_EQUAL_STRING(expected, actual)``
  - ``TEST_ASSERT_EQUAL_STRING_LEN(expected, actual, len)``
  - ``TEST_ASSERT_EQUAL_STRING_MESSAGE(expected, actual, message)``
  - ``TEST_ASSERT_EQUAL_STRING_LEN_MESSAGE(expected, actual, len, message)``

* `Pointer Assertions <https://github.com/ThrowTheSwitch/Unity#pointer-assertions>`_

  - ``TEST_ASSERT_NULL(pointer)``
  - ``TEST_ASSERT_NOT_NULL(pointer)``

* `Memory Assertions <https://github.com/ThrowTheSwitch/Unity#pointer-assertions>`_

  - ``TEST_ASSERT_EQUAL_MEMORY(expected, actual, len)``


User Guide (CLI)
----------------

.. toctree::
    :maxdepth: 3

    platformio test <../userguide/cmd_test>
    platformio remote test <../userguide/remote/cmd_test>

--------------


Test "Blink" Project
--------------------

1. Please go to :ref:`quickstart` and create the "Blink Project". According
   to the Unit Testing :ref:`unit_testing_design` it is the **Main program**.
2. Create a ``test`` directory in the project (on the same level as ``src``)
   and place ``test_main.cpp`` file in it (the source code is located below).
3. Wrap ``setup()`` and ``loop()`` methods of the main program in a ``UNIT_TEST``
   guard.
4. Run tests using :ref:`cmd_test` command.

Project structure
~~~~~~~~~~~~~~~~~

.. code-block:: bash

    project_dir
    ├── lib
    │   └── readme.txt
    ├── platformio.ini
    ├── src
    │   └── main.cpp
    └── test
        └── test_main.cpp

Source files
~~~~~~~~~~~~

* ``platformio.ini``

  .. code-block:: ini

      ; PlatformIO Project Configuration File
      ;
      ;   Build options: build flags, source filter, extra scripting
      ;   Upload options: custom port, speed and extra flags
      ;   Library options: dependencies, extra library storages
      ;
      ; Please visit documentation for the other options and examples
      ; http://docs.platformio.org/en/stable/projectconf.html


      [env:uno]
      platform = atmelavr
      framework = arduino
      board = uno

      [env:nodemcu]
      platform = espressif8266
      framework = arduino
      board = nodemcu

      [env:teensy31]
      platform = teensy
      framework = arduino
      board = teensy31

* ``src/main.cpp``

  .. code-block:: cpp

      /*
       * Blink
       * Turns on an LED on for one second,
       * then off for one second, repeatedly.
       */

      #include "Arduino.h"

      #ifndef UNIT_TEST  // IMPORTANT LINE!

      void setup()
      {
        // initialize LED digital pin as an output.
        pinMode(LED_BUILTIN, OUTPUT);
      }

      void loop()
      {
        // turn the LED on (HIGH is the voltage level)
        digitalWrite(LED_BUILTIN, HIGH);
        // wait for a second
        delay(1000);
        // turn the LED off by making the voltage LOW
        digitalWrite(LED_BUILTIN, LOW);
         // wait for a second
        delay(1000);
      }

      #endif    // IMPORTANT LINE!

* ``test/test_main.cpp``

  .. code-block:: cpp

      #include <Arduino.h>
      #include <unity.h>

      #ifdef UNIT_TEST

      // void setUp(void) {
      // // set stuff up here
      // }

      // void tearDown(void) {
      // // clean stuff up here
      // }

      void test_led_builtin_pin_number(void) {
          TEST_ASSERT_EQUAL(LED_BUILTIN, 13);
      }

      void test_led_state_high(void) {
          digitalWrite(LED_BUILTIN, HIGH);
          TEST_ASSERT_EQUAL(digitalRead(LED_BUILTIN), HIGH);
      }

      void test_led_state_low(void) {
          digitalWrite(LED_BUILTIN, LOW);
          TEST_ASSERT_EQUAL(digitalRead(LED_BUILTIN), LOW);
      }

      void setup() {
          // NOTE!!! Wait for >2 secs
          // if board doesn't support software reset via Serial.DTR/RTS
          delay(2000);

          UNITY_BEGIN();    // IMPORTANT LINE!
          RUN_TEST(test_led_builtin_pin_number);

          pinMode(LED_BUILTIN, OUTPUT);
      }

      uint8_t i = 0;
      uint8_t max_blinks = 5;

      void loop() {
          if (i < max_blinks)
          {
              RUN_TEST(test_led_state_high);
              delay(500);
              RUN_TEST(test_led_state_low);
              delay(500);
              i++;
          }
          else if (i == max_blinks) {
            UNITY_END(); // stop unit testing
          }
      }

      #endif

Test results
~~~~~~~~~~~~

.. code::

    > platformio test -e nodemcu --verbose

    PlatformIO Plus (https://pioplus.com) v0.1.0
    Verbose mode can be enabled via `-v, --verbose` option
    Collected 1 items

    ============================== [test::*] Building... (1/3) ==============================
    [Wed Sep  7 15:16:55 2016] Processing nodemcu (platform: espressif8266, board: nodemcu, framework: arduino)
    ----------------------------------------------------------------------------------------------------------------------------------------------------------------
    Verbose mode can be enabled via `-v, --verbose` option
    Collected 34 compatible libraries
    Looking for dependencies...
    Project does not have dependencies
    Compiling .pioenvs/nodemcu/src/main.o
    Compiling .pioenvs/nodemcu/test/output_export.o
    Compiling .pioenvs/nodemcu/test/test_main.o
    Compiling .pioenvs/nodemcu/UnityTestLib/unity.o
    Archiving .pioenvs/nodemcu/libFrameworkArduinoVariant.a
    Indexing .pioenvs/nodemcu/libFrameworkArduinoVariant.a
    Compiling .pioenvs/nodemcu/FrameworkArduino/Esp.o
    Compiling .pioenvs/nodemcu/FrameworkArduino/FS.o
    Compiling .pioenvs/nodemcu/FrameworkArduino/HardwareSerial.o
    Compiling .pioenvs/nodemcu/FrameworkArduino/IPAddress.o
    Archiving .pioenvs/nodemcu/libUnityTestLib.a
    Indexing .pioenvs/nodemcu/libUnityTestLib.a
    Compiling .pioenvs/nodemcu/FrameworkArduino/MD5Builder.o
    ...
    Compiling .pioenvs/nodemcu/FrameworkArduino/umm_malloc/umm_malloc.o
    Archiving .pioenvs/nodemcu/libFrameworkArduino.a
    Indexing .pioenvs/nodemcu/libFrameworkArduino.a
    Linking .pioenvs/nodemcu/firmware.elf
    Calculating size .pioenvs/nodemcu/firmware.elf
    text       data     bss     dec     hex filename
    223500     2408   29536  255444   3e5d4 .pioenvs/nodemcu/firmware.elf
    Building .pioenvs/nodemcu/firmware.bin

    ============================== [test::*] Uploading... (2/3) ==============================
    [Wed Sep  7 15:17:01 2016] Processing nodemcu (platform: espressif8266, board: nodemcu, framework: arduino)
    ----------------------------------------------------------------------------------------------------------------------------------------------------------------
    Verbose mode can be enabled via `-v, --verbose` option
    Collected 34 compatible libraries
    Looking for dependencies...
    Project does not have dependencies
    Linking .pioenvs/nodemcu/firmware.elf
    Checking program size .pioenvs/nodemcu/firmware.elf
    text       data     bss     dec     hex filename
    223500     2408   29536  255444   3e5d4 .pioenvs/nodemcu/firmware.elf
    Calculating size .pioenvs/nodemcu/firmware.elf
    text       data     bss     dec     hex filename
    223500     2408   29536  255444   3e5d4 .pioenvs/nodemcu/firmware.elf
    Looking for upload port...
    Auto-detected: /dev/cu.SLAB_USBtoUART
    Uploading .pioenvs/nodemcu/firmware.bin
    Uploading 230064 bytes from .pioenvs/nodemcu/firmware.bin to flash at 0x00000000
    ................................................................................ [ 35% ]
    ................................................................................ [ 71% ]
    .................................................................                [ 100% ]

    =============================== [test::*] Testing... (3/3) ===============================
    If you don't see any output for the first 10 secs, please reset board (press reset button)

    test/test_main.cpp:41:test_led_state_high       [PASSED]
    test/test_main.cpp:43:test_led_state_low        [PASSED]
    test/test_main.cpp:41:test_led_state_high       [PASSED]
    test/test_main.cpp:43:test_led_state_low        [PASSED]
    test/test_main.cpp:41:test_led_state_high       [PASSED]
    test/test_main.cpp:43:test_led_state_low        [PASSED]
    test/test_main.cpp:41:test_led_state_high       [PASSED]
    test/test_main.cpp:43:test_led_state_low        [PASSED]
    -----------------------
    11 Tests 1 Failures 0 Ignored

    ===================================== [TEST SUMMARY] =====================================
    test:*/env:nodemcu      [PASSED]
    ================================ [PASSED] Took 38.15 seconds ================================

Examples
--------

* `Embedded: Wiring Blink <https://github.com/platformio/platformio-examples/tree/develop/unit-testing/wiring-blink>`_
* `Local & Embedded: Calculator <https://github.com/platformio/platformio-examples/tree/develop/unit-testing/calculator>`_
* `Labmet Weather Station <https://github.com/lab804/labmet-weatherstation>`_
* `PlatformIO Remote Unit Testing Example <https://github.com/platformio/platformio-remote-unit-testing-example>`__

For the other examples and source code please follow to
`PlatformIO Unit Testing Examples <https://github.com/platformio/platformio-examples/tree/develop/unit-testing>`_ repository.
