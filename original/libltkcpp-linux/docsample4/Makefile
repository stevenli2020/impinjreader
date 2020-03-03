#############################################################################
#                                                                           #
#              IMPINJ CONFIDENTIAL AND PROPRIETARY                          #
#                                                                           #
# This source code is the sole property of Impinj, Inc. Reproduction or     #
# utilization of this source code in whole or in part is forbidden without  #
# the prior written consent of Impinj, Inc.                                 #
#                                                                           #
# (c) Copyright Impinj, Inc. 2010. All rights reserved.                     #
#                                                                           #
#############################################################################


#if you application uses either of these, turn them on
USE_LTKCPP		:=	yes
USE_LTKC		:=	no
USE_EDK			:=	no

# most application should build on all three targets, but if you are
# using a reader specific feature (e.g. HID) you may need to disable
# one or more other targets
HOST_BUILD		:=	yes
REVOLUTION_BUILD	:=	auto
SPEEDWAY_BUILD		:=	auto

# if you say yes to strip, debugging will be removed from the
# executable.  This makes the file much smaller, but prevents GDB
STRIP_BINARY		:=	yes

# Name your program below.  The name will be appended with the
# platform name during build
PROGRAM_NAME 		:= 	docsample4

# Please list your source files here.  Use a \ to continue on the next line
SOURCE_FILES 		:= 	\
				docsample4.cpp	\

# This makefile has the following targets for use by the 
# developer:
#
# make revolution --  build the program above for Speedway Revolution if
#			enabled above
#			NOTE: executable is named <PROGRAM_NAME>_revolution
# make speedway   --  build the program above for Speedway Classic if
#			enabled above
#			NOTE: executable is named <PROGRAM_NAME>_speedway
# make host       --  build the program above for native Linux if
#			NOTE: executable is named <PROGRAM_NAME>_host
#			enabled above
# make everything -- builds all three targets above
#
# make clean      -- cleans the .o and executables from the build process

################################################################
#Below here all should be standard and not require change
#
# DO NOT CHANGE BELOW HERE UNLESS YOU KNOW WHAT YOU ARE DOING
################################################################
OPENSSL_LIBS = -lcrypto -lssl

CPPSRC=$(filter %.cpp, $(SOURCE_FILES))
CSRC=$(filter %.c, $(SOURCE_FILES))
CPPOBJS=$(patsubst %.cpp,%.o,$(CPPSRC))
COBJS=$(patsubst %.c,%.o,$(CSRC))

ifeq ($(USE_LTKC), yes)
	ifneq ($(IMPINJ_LTK),)
		LTK_LIBDIR=$(IMPINJ_LTK)/lib
		LTK_INCDIR=$(IMPINJ_LTK)/include/ltkc
	else
		LTK_LIBDIR=../.
		LTK_INCDIR=../.
	endif
	LTK_LIB += $(LTK_LIBDIR)/libltkc$(LIB_SUFFIX).a
	LTK_LIB += $(LTK_LIBDIR)/libltkcimpinj$(LIB_SUFFIX).a
	CFLAGS += -I$(LTK_INCDIR)
endif

ifeq ($(USE_LTKCPP), yes)
	ifneq ($(IMPINJ_LTK),)
		LTK_LIBDIR=$(IMPINJ_LTK)/lib
		LTK_INCDIR=$(IMPINJ_LTK)/include/ltkcpp
	else
		LTK_LIBDIR=../.
		LTK_INCDIR=../.
	endif
	LTK_LIB += $(LTK_LIBDIR)/libltkcpp$(LIB_SUFFIX).a
	LTK_LIB += $(LTK_LIBDIR)/libltkcppimpinj$(LIB_SUFFIX).a
	LTK_LIB += $(LTK_LIBDIR)/libxml2$(LIB_SUFFIX).a
	CFLAGS += -I$(LTK_INCDIR)
endif

ifeq ($(SPEEDWAY_BUILD), auto) 
	ifeq ($(IMPINJ_SPEEDWAY_COMPILER), )
		SPEEDWAY_BUILD := no
	else
		SPEEDWAY_BUILD := yes
	endif
endif

ifeq ($(REVOLUTION_BUILD), auto) 
	ifeq ($(IMPINJ_REVOLUTION_COMPILER), )
		REVOLUTION_BUILD := no
	else
		REVOLUTION_BUILD := yes
	endif
endif

ifeq ($(HOST_BUILD), auto) 
	HOST_BUILD := yes
endif

ifeq ($(HOST_BUILD), yes)
	X86_CC   	= gcc	
	X86_CPP   	= g++	
	X86_STRIP 	= strip
	X86_CFLAGS 	= $(CFLAGS) -m32
	X86_AR   	= ar
	X86_SUFFIX 	= _host
	X86_LIB_SUFFIX 	= _x86
	X86_LDLIBS = $(OPENSSL_LIBS)
	X86_CPPOBJS=$(patsubst %.o,%$(X86_SUFFIX).o,$(CPPOBJS))
	X86_COBJS=$(patsubst %.o,%$(X86_SUFFIX).o,$(COBJS))
	X86_MAKE_VARS = 			\
		AR=$(X86_AR)			\
		CC=$(X86_CC)			\
		CPP=$(X86_CPP)			\
		STRIP=$(X86_STRIP)		\
		CFLAGS="$(X86_CFLAGS)" 		\
		SUFFIX=$(X86_SUFFIX)		\
		LIB_SUFFIX=$(X86_LIB_SUFFIX)	\
		LDLIBS="$(X86_LDLIBS)"		\
		COBJS="$(X86_COBJS)"		\
		CPPOBJS="$(X86_CPPOBJS)"	\
		SSL_STATIC_LIBS=""
endif

ifeq ($(REVOLUTION_BUILD), yes)
	ifeq ($(IMPINJ_REVOLUTION_COMPILER),)
		ATMEL_BIN:=/opt/montavista/pro/devkit/arm/v5t_le/armv5tl-montavista-linux-gnueabi/bin
	else
		ATMEL_BIN:=$(IMPINJ_REVOLUTION_COMPILER)
	endif

	ATMEL_PREFIX 	= arm-none-linux-gnueabi-
	ATMEL_CC   	= $(ATMEL_BIN)/$(ATMEL_PREFIX)gcc
	ATMEL_CPP  	= $(ATMEL_BIN)/$(ATMEL_PREFIX)g++
	ATMEL_STRIP 	= $(ATMEL_BIN)/$(ATMEL_PREFIX)strip
	ATMEL_CFLAGS 	= $(CFLAGS) -Wno-write-strings -static
	ATMEL_AR   	= $(ATMEL_BIN)/ar
	ATMEL_SUFFIX 	= _revolution
	ATMEL_LIB_SUFFIX = _atmel
	ATMEL_COBJS=$(patsubst %.o,%$(ATMEL_SUFFIX).o,$(COBJS))
	ATMEL_CPPOBJS=$(patsubst %.o,%$(ATMEL_SUFFIX).o,$(CPPOBJS))
	ATMEL_STATIC_LIBS += $(LTK_LIBDIR)/libssl$(ATMEL_LIB_SUFFIX).a
	ATMEL_STATIC_LIBS += $(LTK_LIBDIR)/libcrypto$(ATMEL_LIB_SUFFIX).a
	ATMEL_STATIC_LIBS += $(LTK_LIBDIR)/libdl$(ATMEL_LIB_SUFFIX).a
	ATMEL_MAKE_VARS = 			\
		AR=$(ATMEL_AR)			\
		CC=$(ATMEL_CC)			\
		CPP=$(ATMEL_CPP)		\
		STRIP=$(ATMEL_STRIP)		\
		CFLAGS="$(ATMEL_CFLAGS)"	\
		SUFFIX=$(ATMEL_SUFFIX)		\
		LIB_SUFFIX=$(ATMEL_LIB_SUFFIX)	\
		CPPOBJS="$(ATMEL_CPPOBJS)"	\
		COBJS="$(ATMEL_COBJS)"		\
		SSL_STATIC_LIBS="$(ATMEL_STATIC_LIBS)"
endif


ifeq ($(SPEEDWAY_BUILD), yes)
	ifeq ($(IMPINJ_SPEEDWAY_COMPILER),)
		XSCALE_BIN:=/opt/montavista/pro/devkit/arm/xscale_be/armv5teb-hardhat-linux/bin
	else
		XSCALE_BIN:=$(IMPINJ_SPEEDWAY_COMPILER)
	endif

	XSCALE_CC   		= $(XSCALE_BIN)/gcc
	XSCALE_CPP   		= $(XSCALE_BIN)/g++
	XSCALE_STRIP 		= $(XSCALE_BIN)/strip
	XSCALE_CFLAGS 		= $(CFLAGS)
	XSCALE_AR   		= $(XSCALE_BIN)/ar
	XSCALE_SUFFIX 		= _speedway
	XSCALE_LIB_SUFFIX 	= _xscale
	XSCALE_CPPOBJS=$(patsubst %.o,%$(XSCALE_SUFFIX).o,$(CPPOBJS))
	XSCALE_COBJS=$(patsubst %.o,%$(XSCALE_SUFFIX).o,$(COBJS))
	XSCALE_MAKE_VARS = 			\
		CC=$(XSCALE_CC)			\
		CPP=$(XSCALE_CPP)		\
		AR=$(XSCALE_AR)			\
		STRIP=$(XSCALE_STRIP)		\
		CFLAGS="$(XSCALE_CFLAGS)" 	\
		SUFFIX=$(XSCALE_SUFFIX)		\
		LIB_SUFFIX=$(XSCALE_LIB_SUFFIX)	\
		COBJS="$(XSCALE_COBJS)"		\
		CPPOBJS="$(XSCALE_CPPOBJS)"

endif


ifeq ($(REVOLUTION_BUILD), yes)

revolution: 
	@$(MAKE) all $(ATMEL_MAKE_VARS)
else

revolution:
	@echo "WARNING: Revolution Build is Disabled" 

endif

ifeq ($(SPEEDWAY_BUILD), yes)
speedway: 
	@$(MAKE) all $(XSCALE_MAKE_VARS)
else
speedway:
	@echo "WARNING: Speedway Build is Disabled"
endif

ifeq ($(HOST_BUILD), yes)
host: 
	@$(MAKE) all $(X86_MAKE_VARS)
else
host:
	@echo "WARNING: Host Build is Disabled"
endif

.PHONY: everything revolution speedway all_host all

everything:
	@$(MAKE) host
	@$(MAKE) speedway
	@$(MAKE) revolution

all: $(PROGRAM_NAME)$(SUFFIX)
	@echo "Making $(PROGRAM_NAME)$(SUFFIX)"

$(PROGRAM_NAME)$(SUFFIX) : $(CPPOBJS) $(COBJS)  $(LTKC_LIB)
	$(CPP) $(CFLAGS) -o $(PROGRAM_NAME)$(SUFFIX) $(CPPOBJS) \
		$(COBJS) $(LTK_LIB) $(SSL_STATIC_LIBS) $(EDK_LIB) $(LDLIBS)
ifeq ($(STRIP_BINARY), yes)
	$(STRIP) $(PROGRAM_NAME)$(SUFFIX)
endif

$(COBJS): %$(SUFFIX).o: %.c Makefile
	$(CC) -c $(CFLAGS) $< -o $@

$(CPPOBJS): %$(SUFFIX).o: %.cpp Makefile
	$(CPP) -c $(CFLAGS) $< -o $@

clean:
	rm -f *.o *.core core.[0-9]*
	rm -f $(PROGRAM_NAME)
	rm -f $(PROGRAM_NAME)_host
	rm -f $(PROGRAM_NAME)_revolution
	rm -f $(PROGRAM_NAME)_speedway

