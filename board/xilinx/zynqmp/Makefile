#
# (C) Copyright 2014 - 2016 Xilinx, Inc.
# Michal Simek <michal.simek@xilinx.com>
#
# SPDX-License-Identifier:	GPL-2.0+
#

obj-y	:= zynqmp.o

hw-platform-y :=$(shell echo $(CONFIG_DEFAULT_DEVICE_TREE))

init-objs := $(if $(wildcard $(srctree)/$(src)/$(hw-platform-y)/psu_init_gpl.c),\
	$(hw-platform-y)/psu_init_gpl.o)

ifeq ($(init-objs),)
ifneq ($(wildcard $(srctree)/$(src)/psu_init_gpl.c),)
init-objs := psu_init_gpl.o
$(if $(CONFIG_SPL_BUILD),\
$(warning Put custom psu_init_gpl.c/h to board/xilinx/zynqmp/custom_hw_platform/))
endif
endif

ifdef_any_of = $(filter-out undefined,$(foreach v,$(1),$(origin $(v))))

ifneq ($(call ifdef_any_of, CONFIG_ZYNQMP_PSU_INIT_ENABLED CONFIG_SPL_BUILD),)
obj-y += $(init-objs)
endif

obj-$(CONFIG_ZYNQ_SDHCI) += tap_delays.o

# Suppress "warning: function declaration isn't a prototype"
CFLAGS_REMOVE_psu_init_gpl.o := -Wstrict-prototypes

# To include xil_io.h
CFLAGS_psu_init_gpl.o := -I$(srctree)/$(src)

# To suppress "warning: cast to pointer from integer of different size"
CFLAGS_psu_init_gpl.o += -Wno-int-to-pointer-cast
