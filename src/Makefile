#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************
#------------------------------------------------------------------------------
# Dual platform Makefile tha supports a host environment (LINUX) and the
# target embedded system MSP432 (ARM)
#
# Use: make build PLATFORM=MSP432
#      make build PLATFORM=HOST
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

ifeq ($(PLATFORM),HOST)
		CC = gcc
		LD = ld
		LDFLAGS =  
		OBJDUMP = objdump
else
		CC = arm-none-eabi-gcc
		LD = arm-none-eabi-ld
		LINKER_FILE = ../msp432p401r.lds
		LDFLAGS = -T $(LINKER_FILE)
		CPU = cortex-m4
		ARMFLAGS = -mcpu=$(CPU) -mthumb -march=armv7e-m -mfloat-abi=hard\
			   -mfpu=fpv4-sp-d16 --specs=nosys.specs
		OBJDUMB = arm-none-eabi-objdumb
endif

TARGET = c1m2
CFLAGS = -Wall -Werror -g -O0 -std=c99 -D$(PLATFORM) $(ARMFLAGS) 
CPPFLAGS = -E

OBJS = $(SOURCES:.c=.o)
PREPRO = $(SOURCES:.c=.i)
ASMS = $(SOURCES:.c=.asm)
DEPS = $(SOURCES:.c=.d)

all: build

$(TARGET).out : $(OBJS)
	$(LD) -lc $^ -o $@ $(LDFLAGS)

%.o : %.c
	$(CC) -o $@ -c $< $(INCLUDES) $(CFLAGS)

%.i : %.c
	$(CC) -o $@ $< $(INCLUDES) $(CFLAGS) $(CPPFLAGS)

%.asm : %.c
	$(CC) -o $@ -S $< $(INCLUDES) $(CFLAGS)

%.d : %.c
	$(CC) -o $@ -M $< $(INCLUDES) $(CFLAGS)


.PHONY: compile-all
compile-all: $(OBJS)

.PHONY: build
build: $(DEPS) $(TARGET).out   

.PHONY: info
info: 
	$(info $(SOURCES))
	$(info $(INCLUDES))

.PHONY: clear
clear: 
	rm $(TARGET).out $(OBJS) $(ASMS) $(PREPRO) $(DEPS) $(TARGET).asm \
		$(TARGET).map


























