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

.. _cmd_remote:

PIO Remote™ CLI
===============

:ref:`pio_remote` allows you to work remotely with devices from
*Anywhere In The World*. No matter where are you now! Run a small and
cross-platform :ref:`cmd_remote_agent` on a remote machine and you are able to
list active devices (wireless + wired), to upload firmware (program),
to process remote unit tests, or to start remote debugging session via
**Remote Serial Port Monitor**.

Using :ref:`pio_remote` you can share your devices with friends or team. In
combination with Cloud IDE, you can create awesome things at any time when
inspiration comes to you.

You should have :ref:`cmd_account` to work with :ref:`pio_remote`.
A registration is **FREE**.

For detailed information please follow to :ref:`pio_remote` page.

To print all available commands and options use:

.. code-block:: bash

    pio remote --help
    platformio remote --help
    platformio remote COMMAND --help

    # run command on the specified PIO Remote Agents
    platformio remote --agent NAME_1 --agent NAME_N COMMAND


.. toctree::
    :maxdepth: 2

    cmd_agent
    cmd_device
    cmd_run
    cmd_test
    cmd_update
