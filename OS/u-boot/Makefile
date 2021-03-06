#
# $Id$
#
# Red Pitaya U-Boot Makefile 
#
# Produces:
#   1. U-Boot ELF binary.
#   2. U-Boot environment tools.
#


UBOOT_GIT := /var/git/u-boot-xlnx.git
UBOOT_TAG := xilinx-v14.6.01
UBOOT_DIR := u-boot-xlnx
UBOOT := $(UBOOT_DIR)/u-boot
PATCH := patches
ENVTOOLS := envtools
INSTALL_DIR ?= .
CROSS_COMPILE ?= arm-xilinx-eabi-
HOST_COMPILE ?= arm-xilinx-linux-gnueabi-

.PHONY : all install clean envtools

all: $(UBOOT_DIR)
	make -C $(UBOOT_DIR) arch=ARM CROSS_COMPILE=$(CROSS_COMPILE) all
	make -C $(UBOOT_DIR) arch=ARM HOSTCC=$(HOST_COMPILE)gcc HOSTSTRIP=$(HOST_COMPILE)strip env

$(UBOOT_DIR):
	mkdir $(UBOOT_DIR)
	cd $(UBOOT_DIR); git archive --remote $(UBOOT_GIT) $(UBOOT_TAG) | tar x
	cat $(PATCH)/*.patch | patch -p1 -d $(UBOOT_DIR)
	make -C $(UBOOT_DIR) arch=ARM zynq_red_pitaya_config

install: all
	cp $(UBOOT_DIR)/u-boot $(INSTALL_DIR)/u-boot.elf
	mkdir -p $(INSTALL_DIR)/bin/
	mkdir -p $(INSTALL_DIR)/etc/
	cp $(UBOOT_DIR)/tools/env/fw_printenv $(INSTALL_DIR)/bin
	cp $(UBOOT_DIR)/tools/env/fw_printenv $(INSTALL_DIR)/bin/fw_setenv
	cp $(ENVTOOLS)/fw_env.config $(INSTALL_DIR)/etc

clean:
	-make -C $(UBOOT_DIR) CROSS_COMPILE=$(CROSS_COMPILE) arch=ARM clean

distclean:
	rm -rf $(UBOOT_DIR)

