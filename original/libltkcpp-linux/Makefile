#############################################################################
#                                                                           #
#              IMPINJ CONFIDENTIAL AND PROPRIETARY                          #
#                                                                           #
# This source code is the sole property of Impinj, Inc. Reproduction or     #
# utilization of this source code in whole or in part is forbidden without  #
# the prior written consent of Impinj, Inc.                                 #
#                                                                           #
# (c) Copyright Impinj, Inc. 2007,2010. All rights reserved.                #
#                                                                           #
#############################################################################

SAMPLE_DIRS = docsample1 docsample2 docsample3 docsample4 docsample5
CUST_DIRS = custom_select_1 custom_select_2
SUBDIRS = $(SAMPLE_DIRS) $(CUST_DIRS)

.PHONY: everything
everything:
	for dir in $(SUBDIRS) ; do \
		make -C $$dir $@; \
	done

.PHONY: all_x86
all_x86:
	for dir in $(SAMPLE_DIRS)
		make -C $$dir host; \
	done

.PHONY: all_atmel
all_atmel:
	for dir in $(SAMPLE_DIRS)
		make -C $$dir revolution; \
	done

.PHONY: clean
clean:
	for dir in $(SUBDIRS) ; do \
		make -C $$dir $@; \
	done

