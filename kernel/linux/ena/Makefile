ENA_COM_PATH=../common/ena_com/

obj-m += ena.o
ena-objs := ena_netdev.o \
		ena_ethtool.o \
		$(ENA_COM_PATH)/ena_com.o \
		$(ENA_COM_PATH)/ena_eth_com.o

ena-$(CONFIG_SYSFS) += ena_sysfs.o

ccflags-y := -I$(src)
ccflags-y += -I$(src)/$(ENA_COM_PATH)/


BUILD_KERNEL ?= $(shell uname -r)

ccflags-y += -Wfatal-errors

GENERATED_EXISTS := $(shell if [ -d include/generated ]; then echo True; fi)

ifneq ($(strip $(GENERATED_EXISTS)),)
  IS_UEK=$(shell grep uek include/generated/utsrelease.h)

  ifneq ($(strip $(IS_UEK)),)
    IS_3_8_13=$(shell grep -F 3.8.13 include/generated/utsrelease.h)

    ifneq ($(strip $(IS_3_8_13)),)
      ccflags-y += -DUEK3_RELEASE
    else
      $(error only UEK3 with kernel version 3.8.13 is suppported) 
    endif
  endif
endif

all:
	make -C /lib/modules/$(BUILD_KERNEL)/build M=$(CURDIR) modules

clean:
	make -C /lib/modules/$(BUILD_KERNEL)/build M=$(CURDIR) clean
