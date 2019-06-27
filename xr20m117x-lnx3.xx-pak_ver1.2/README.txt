Exar I2c Serial Driver
======================
Version 1.2, 9/11/2015

This driver will work with any I2C/SPI UART function in these Exar devices:
	XR20M1170/XR20M1172


Integrated into your system
---------------------------
1.Please copy the driver file to the kernel directory:$kernel_src\drivers\tty\serial

2.Please add the followed txt into the kernel file:$kernel_src\drivers\tty\serial\Konfig
config SERIAL_XR20M117X
	tristate "SERIAL_XR20M117X serial support"
	depends on I2C
	select SERIAL_CORE
	select REGMAP_I2C if I2C
	help
	  This selects support for SERIAL_XR20M117X serial ports.
	 
3.Add the follow define into the $kernel_src\drivers\tty\serial\Makefile for compile the driver.
obj-$(CONFIG_SERIAL_XR20M117X) += xrm117x.o

4.Run the make menuconfig and select the XRM117x serial support at the driver/tty/serial and save the config.

5 if you use the I2c mode:

Remove the macro define "#define USE_SPI_MODE  1" at the file top position.

Define the i2c_board_info object on your board file similar the follow:
static struct i2c_board_info smdk4x12_i2c_devs1[] __initdata = {
	
	    { 
			I2C_BOARD_INFO("xrm117x", 0x30), 
		  	.platform_data  = &ft5x0x_pdata,
		  	.irq            = IRQ_EINT(14),
	     }


   
};
6.if you use the SPI mode:

Add the macro define "#define USE_SPI_MODE  1" at the file top position.

Define the spi0_board_info object on your board file similar the follow:
static struct spi_board_info spi0_board_info[] __initdata = {
	{
		.modalias = "xrm117x_spi",
		.platform_data = NULL,
		.max_speed_hz = 10*1000,
		.bus_num = 0,
		.chip_select = 0,
		.mode = SPI_MODE_0,
		.controller_data = &spi0_csi[0],
		.irq		= IRQ_EINT(25),
	}
};

* if you need change the xtal freq, you can modify it at about line:1087
  freq = 14745600;

Technical Support
-----------------
Send any technical questions/issues to uarttechsupport@exar.com. 

