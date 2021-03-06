ToDo
====

.. note::

  This page is taken from TODO file in source root. Some points are maybe obsolete
  or done. This is to check!
  
General
-------

- Running simulavr as part of the WinAVR tool chain.
- Grouping of identical AVR hardware, such as: What kind of timers
  are around 8bit vs. 16bit, functions, PWM...
- Export additional hardware (LCDs etc.) to verilog
- Proper teardown:
  Currently, there are various points in the simulator where a simple
  exit(x) is performed. This may hurt additional instrumentation software
  which may need to close files or it may hurt any external program
  which is controlling simulavr.
- State saving support:
  Saving/restoring the complete state of the simulation (this needs
  a lot of infrastructure) *Is this needed?*
- Clean up the private/protected/public declaration scopes
  for the various classes. This is quite inconsistent.
  
Host specific issues
--------------------

- Cygwin: Basic Simulator Functionality Works.  Need to check Tcl/X.
- Mingw: Uses BSD sockets.  Need to have WinSock as alternative
- check for makeinfo/texi2html program, else a full build will fail!

Feature parity with avrtest
---------------------------

Known issues:

- Verify gcc test suite can be run with avrtest
  
Feature parity with old simulavr
--------------------------------

Known issues:

- avr-libc mostly tests with AT90S8515 and ATmega128 so most is OK
- Models not yet supported (this is a historical list, many modern models
  are also not supported!):

  - AT90S2313 \-
  - AT90S4414 - essentially AT90S8515 w/less memory so easy
  - AT90S1200 - different, can be ignored as obsolete
  - ATmega103 - can be ignored as obsolete
  - range of AT43USBxxx controllers - can be ignored as replaced (sorta)
    by the AT90USB* / ATmega???U? devices.
    
- old simulavr has a core dump feature
  
Configure issues
----------------

- Joel's Cygwin has texi2pdf but not a valid TeX install.  It configures
  OK but fails when building documentation.
- libiconv.a and libintl.a are not needed on Fedora 10 but required on
  Cygwin.  They are forced in on Cygwin now.  Can we know dynamically
  if these are needed?
  
Known limitations of current CPU models
---------------------------------------

- TWI Serial Interface missing
- Analog to Digital Converter Subsystem need to be validated
- Boot Loader Support (fuses, different reset vector, switching interrupt
  vector table, ...) not supported
- Real Time Clock missing
- Watchdog Timer status unclear
- Sleep-command not working
- Reset-pin is not available. Also different reset reasons are not supported
- With activating the Tx-Pin of an UART the DDR-Register is not set properly
  to output. Workaround: Set the Pin's default value to PULLUP. While the
  Pin behaves as Open Colletor (pulls down only) the pull-up "resistor" lets
  the system run as it should.
- There are always 64kByte of external memory automatically attached to the
  Mega128. Different memory sizes are not supported.
- Segments different than .text, .data, .bss are not supported

Known limitation of other/new CPU models
----------------------------------------

- While Atmel changed some function details of the EEPROM, Watchdog Timer,
  Timer Subsystem, ADC, and USART / USI these subsystems have identical
  names but different functions. Therefore adding a new CPU needs a detailed
  analysis of the functions of each IO-subsystem
- CPUs with 24bit program counter are not supported
- USI-subsystem is missing
- CAN-subsystem is missing
- USB subsystem is missing
- X-Mega specifics are missing

Known issues of the simulator
-----------------------------

- LCD subsystem is simlified to

  - only a 4*20 character type is available
  - the timing is fixed as described for the 5V-versions
  - only the 4 bit interface is supported. At start-up the commands are
    interpreted as if an eight bit interface is available (one write cycle
    per command). After finishing the initialization switching to the four
    bit interface is permitted at any time.
  - the graphic representation is character based. Display of of characters
     follows the rules of your display, not of the LCD character generator.
  - loadable characters are not supported.
  - reading of display is not supported.
  - reading of busy flag does not give the current address in the lower bits.
  - scrolling not supported.
  - shift right / left of the display content is not supported.
  - only one character set is supported - based on your diplay font.

- Scope subsystem is an empty frame
- Serial Rx/Tx is simplfied to

  - only 8 bit data transmission is supported
  - 9 bit handling (MPM) is not supported

- Waiting at a breakpoint for a GDB user input: the active wait-loop should
  be replaced by something using less CPU power.
  
Testing
-------

- All tests need to be integrated into a nice regression test suite (Maybe
  python unittest?), this is made for some tests, not for all.
  Right now, the tests are split up and there is a custom made regression
  test suite for testing the handling of instructions in the core.
  
Tracing/Dumping
---------------

- Make the device names configurable and not only a simple index into a list.
  Do not update bxxxx values in trace output.
- Implement PCb back.
- Bug, nothing in SREG? Why SREG twice?
- Have an optional constraint on the number of bits in trace_direct (to e.g.
  only trace the necessary stack bits).

