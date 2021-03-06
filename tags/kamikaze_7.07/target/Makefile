# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/target.mk

all: install

$(BIN_DIR):
	mkdir -p $(BIN_DIR)

linux-install: $(BIN_DIR)
image_install: linux-install

IMAGE_DIR:=linux/$(BOARD)-$(KERNEL)/image

download: $(patsubst %,%-download,$(TARGETS-y))
prepare: linux-prepare
compile: linux-compile image_compile
install: linux-install image_install 

ifeq ($(CONFIG_SDK),y)
install: sdk_install
endif
ifeq ($(CONFIG_IB),y)
install: imagebuilder_install
endif

clean: linux-clean sdk-clean imagebuilder-clean image_clean

prereq: FORCE
	$(MAKE) -C linux/$(BOARD)-$(KERNEL) prereq
	$(MAKE) -C $(IMAGE_DIR) prereq

sdk_install: FORCE
	$(MAKE) -C sdk install

image_clean: FORCE
	$(MAKE) -C $(IMAGE_DIR) clean
	rm -f $(BIN_DIR)/openwrt-*
	
image_compile: FORCE
	$(MAKE) -C $(IMAGE_DIR) compile

image_install: image_compile
	$(MAKE) -C $(IMAGE_DIR) install

imagebuilder_install: image_install
	$(MAKE) -C imagebuilder install

%-prereq %-prepare %-download %-clean %-compile %-install %-refresh %-update: FORCE
	$(MAKE) -C $* $(patsubst $*-%,%,$@)
