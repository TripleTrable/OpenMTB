OpenMTB backplane
=================

The backplane consist of three main areas. The bus itself, consisting of
different voltage lines as well as the SPI data lines an CS lines. The other two
areas are power conversion and the mcu control area.

To prevent any problems with power during boot-up, all voltages have to be
enabled with a mosfet by the mcu on the board. This allows the Raspberry-Pi to
first enable 3v3 and 5v to initialize the modules. Once all modules are finished
initializing, the 24v rail is enabled fully powering the system. Also all
voltage rails are measured by the mcu with an INA228/INA238.


.. image:: ./images/backplane-diagram.png



Currently the following LEDs are planned for the status. All are RGB.
These are only the First concepts and it is to assume that everything can and
will change.

* 1 LED for each voltage Rail:
    * Green: Enabled
    * Blue: Disabled
    * Red: Error

* SPI MCU Status
    * Green: Connected
    * Blue: Not Connected
    * Red: SPI error

* Raspberry-Pi Status
    * Green: OK
    * Blue: Not known
    * Red: Error

* Overall System Status
    * Green: OK
    * Blue: Not known
    * Red: Error


For starting the development, the following values are used in the design. All
the values for current are guessed and will change in the future

.. list-table:: Current availabe for the voltage lines
   :widths: 25 25
   :header-rows: 1

   * - Volage
     - Current
   * - 3v3
     - 2A
   * - 5v
     - 5A
   * - 24V 
     - 10A ? (All external power)


Bus
---

The bus will consist of all three (3v3, 5v, 24v) power rails + ground as well as an SPI
lines. The power lines will likely cover multiple pins of the connector to
distribute te current. All connections of the bus are to all modules.

For each module
---------------

These are line, which are only between the Raspberry-Pi and one single module.
These consist of the CS lines for SPI as well a UART connection to programm all
modules mcus. To achieve the necessary 7 ( 6 modules + bacplane) UART
connections. 2 Mux (e.g. SN74LV4052A) are used to split the signals for each
board. 
