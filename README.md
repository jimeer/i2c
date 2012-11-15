Run this program to slow increase the delay of the Broadcom I2C register as follows
sudo ./bcmdel

It can be compiled from the source by:

gcc –o bcmdel bcm_del.c

Synopsys:
The driver does not allow control over the clock speed as it is fixed as a constant in the driver thus:
#define I2C_CLOCK_HZ	100000
Also it can’t be changed outside the driver as it is reset to this value on each transaction. Fortunately the writes of the driver forgot about the ‘DIV’ register and just left it at its default values. The program above simply increases the delay in this register that effectively alters the clock speed ever so slightly but enough for devices that would not normally work to now work.
The 32bit register is split into two halves and the default value is 0x00300030, this has been increased for both sides. Increasing it too much has a worsening effect. For interest the following will get the firmware version for a BV42nn as a 16 bit word
i2cget –y 0 0x31 0xa0 w
The scope trace without running bcmdel and with is shown below. Slowing the clock speed may also have the same effect but unfortunately this is not possible