# Makefile
#
ROOTDIR = ../..
#include $(ROOTDIR)/Rules.make
PUBLIC_INCLUDE_DIR = $(ROOTDIR)/interface/inc
CROSS_COMPILE = arm-none-linux-gnueabi-
APP_LIB_DIR = $(ROOTDIR)/interface/lib
TARGET_FS = $(ROOTDIR)/target/rfs_816x
EXEC_DIR = $(TARGET_FS)/opt/dvr_rdk/ti816x
FC_TOOLS_DIR = $(ROOTDIR)/ti_tools/framework_components/framework_components_3_22_02_08/fctools


TARGET = system_server

C_FLAGS += -Wall -g -O3

CPP_FLAGS += -I$(PUBLIC_INCLUDE_DIR) \
		-I../inc \
		-D_GNU_SOURCE \
		-D_LARGEFILE64_SOURCE \
		-D_FILE_OFFSET_BITS=64 \
                $(XDC_FLAGS)

LD_FLAGS += -lfreetype -lpthread
#LD_FLAGS +=  -lpthread

COMPILE.c = $(CROSS_COMPILE)gcc $(C_FLAGS) $(CPP_FLAGS) -c
LINK.c = $(CROSS_COMPILE)gcc $(LD_FLAGS)

#COMPILE.c = gcc $(C_FLAGS) $(CPP_FLAGS) -c
#LINK.c = gcc $(LD_FLAGS)

RELTARGET = release/$(TARGET)

RELCFLAGS = -fno-strict-aliasing

SOURCES = $(wildcard *.c)
HEADERS = $(wildcard *.h) $(PUBLIC_INCLUDE_DIR)/*.h

RELOBJFILES = $(SOURCES:%.c=release/%.o)

RELLDFLAGS =



.PHONY: clean release install

all:    release
	make clean
	make install

install: release
	install -d $(EXEC_DIR)
	install $(RELTARGET) $(EXEC_DIR)
	ln -sf /tmp/localtime $(TARGET_FS)/usr/share/zoneinfo/localtime
	ln -sf /tmp/localtime $(TARGET_FS)/usr/share/zoneinfo/posixrules
	ln -sf /tmp/localtime $(TARGET_FS)/etc/localtime

release:    $(RELTARGET)

$(RELTARGET):   $(RELOBJFILES) $(APP_LIB_DIR)/file_msg_drv.a $(APP_LIB_DIR)/Appro_interface.a $(APP_LIB_DIR)/alarm_msg_drv.a  $(APP_LIB_DIR)/dm355_gio_util.a  $(FC_TOOLS_DIR)/packages/ti/sdo/linuxutils/cmem/lib/cmem.a470MV
	$(LINK.c) -o $@ $^ $(RELLDFLAGS)


$(RELOBJFILES): release/%.o: %.c $(HEADERS)
	@mkdir -p release
	$(COMPILE.c) $(RELCFLAGS) -o $@ $<

clean:
	-$(RM) -rf release  *.d
