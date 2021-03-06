# 
# Copyright (C) 2006-2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile 6070 2007-01-10 21:21:59Z nbd $

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/host.mk

override MAKEFLAGS=

PKG_OS:=$(shell uname -s)
PKG_CPU:=$(shell uname -m | sed "s/ //g")

IB_NAME:=OpenWrt-ImageBuilder-$(BOARD)-for-$(PKG_OS)-$(PKG_CPU)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(IB_NAME)
IB_KDIR:=$(patsubst $(TOPDIR)/%,$(PKG_BUILD_DIR)/%,$(KERNEL_BUILD_DIR))

all: compile

$(BIN_DIR)/$(IB_NAME).tar.bz2: clean
	rm -rf $(PKG_BUILD_DIR)
	mkdir -p $(IB_KDIR) $(PKG_BUILD_DIR)/staging_dir/host $(PKG_BUILD_DIR)/target
	$(CP) \
		$(INCLUDE_DIR) $(SCRIPT_DIR) \
		$(TOPDIR)/rules.mk $(TOPDIR)/.config \
		./files/Makefile \
		$(TMP_DIR)/.targetinfo \
		$(TMP_DIR)/.packageinfo \
		$(PKG_BUILD_DIR)/
	$(CP) $(PACKAGE_DIR) $(PKG_BUILD_DIR)/packages
	$(CP) $(TOOLCHAIN_DIR)/bin $(PKG_BUILD_DIR)/staging_dir/host/
	$(CP) $(STAGING_DIR_HOST)/bin/* $(PKG_BUILD_DIR)/staging_dir/host/bin/
	mkdir -p $(PKG_BUILD_DIR)/target/linux
	$(CP) $(TOPDIR)/target/linux/$(BOARD) $(PKG_BUILD_DIR)/target/linux/
        ifeq ($(BOARD),brcm47xx)
		$(CP) $(TOPDIR)/target/linux/brcm-2.4 $(PKG_BUILD_DIR)/target/linux/
        endif
	rm -rf \
		$(PKG_BUILD_DIR)/target/linux/*/files \
		$(PKG_BUILD_DIR)/target/linux/*/patches \
		$(PKG_BUILD_DIR)/target/linux/*/*/patches
	-cp $(KERNEL_BUILD_DIR)/* $(IB_KDIR)/ # don't copy subdirectories here
	find $(PKG_BUILD_DIR) -name .svn | $(XARGS) rm -rf
	find $(PKG_BUILD_DIR) -name .git | $(XARGS) rm -rf
	echo -e 'KERNEL:=$(KERNEL)\nLINUX_VERSION:=$(LINUX_VERSION)\nREVISION:=$(REVISION)' \
		>$(PKG_BUILD_DIR)/include/version.mk
	$(TAR) c -C $(BUILD_DIR) $(IB_NAME) | bzip2 -c > $@

download:
prepare:
compile: $(BIN_DIR)/$(IB_NAME).tar.bz2
install: compile

clean: FORCE
	rm -rf $(PKG_BUILD_DIR) $(BIN_DIR)/$(IB_NAME).tar.bz2
