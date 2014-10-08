##########################
# where your kernel sources are
KERNELDIR ?= /lib/modules/`uname -r`/build

GCC = gcc
DEPMOD = /sbin/depmod

# end configuration part
##########################

##########################
# some common switches and defines
DBG     = NO_DEBUG
MOD     = MODVERSIONS
PAR     = PARPORT_SUBSYSTEM
USB     = USB_SUPPORT
PCI     = PCI_SUPPORT
PCIEC   = PCIEC_SUPPORT
DNG     = DONGLE_SUPPORT
ISA     = ISA_SUPPORT
PCC     = PCCARD_SUPPORT
NET     = AUTO_NETDEV_SUPPORT
RT      = NO_RT

VERBOSE = 0

PWD     = $(shell pwd)

ifeq ($(DBG),DEBUG)
VERBOSE = 1
endif

ifneq ($(PCI),PCI_SUPPORT)
PCIEC = NO_PCIEC_SUPPORT
endif

# end switches and defines
##########################

PWD := $(shell pwd)

KERNELSRC ?= /usr/src/linux-headers-$(shell uname -r)
KERNELMODS ?= /lib/modules/$(shell uname -r)

EXTRA_CFLAGS = $(CMODFLAGS)
EXTRA_CFLAGS += -I$(PWD) -D$(DBG) -D$(MOD) -D$(PAR) -D$(USB) -D$(PCI)
EXTRA_CFLAGS += -D$(PCIEC) -D$(ISA) -D$(DNG) -D$(PCC) -D$(NET) -D$(RT)

obj-m := pcan.o

pcan-objs := pcan_main.o pcan_fops.o pcan_fifo.o pcan_filter.o
pcan-objs += pcan_parse.o pcan_sja1000.o pcan_common.o

ifeq ($(PCI),PCI_SUPPORT)
pcan-objs += pcan_pci.o
endif

ifeq ($(PCIEC),PCIEC_SUPPORT)
pcan-objs += pcan_pciec.o 
endif

ifeq ($(ISA),ISA_SUPPORT)
pcan-objs += pcan_isa.o
endif

ifeq ($(DNG),DONGLE_SUPPORT)
pcan-objs += pcan_dongle.o
endif

ifeq ($(PCC),PCCARD_SUPPORT)
pcan-objs += pcan_pccard_kernel.o pcan_pccard.o 
endif

ifeq ($(USB),USB_SUPPORT)
pcan-objs += pcan_usb_core.o pcan_usb.o
pcan-objs += pcan_usbpro.o pcan_timing.o
endif

ifeq ($(NET),NETDEV_SUPPORT)
pcan-objs += pcan_netdev.o
endif

all: module

module: pcan_main.c
	@$(MAKE) -C $(KERNELSRC) SUBDIRS=$(PWD) M=$(PWD) EXTRA_CFLAGS="$(EXTRA_CFLAGS)" V=$(VERBOSE) modules

install: module-install

module-install: pcan.ko
	install -c -o root -g root -m 644 pcan.ko \
	$(KERNELMODS)/kernel/drivers/misc
	$(DEPMOD) -ae

clean:
	rm -rf *.o *.ko *.mod.c .*.cmd .tmp_versions *~ *.a *.so *.d
	rm -rf Module.symvers Module.markers modules.order
