PROJ=$(notdir $(CURDIR))

OBJS=main.o

ELF=$(PROJ).elf

TOOLROOT=/opt/msp430
PREFIX ?= msp430

#TOOLROOT=~/ti/gcc
#PREFIX ?= msp430-elf

GCC_DIR =  $(TOOLROOT)/bin
SUPPORT_FILE_DIRECTORY = $(TOOLROOT)/include

MSPDEBUG=~/work/msp430/mspdebug/mspdebug

DEVICE  = msp430g2553


CC      = $(GCC_DIR)/$(PREFIX)-gcc
LD      = $(GCC_DIR)/$(PREFIX)-gcc
SIZE    = $(GCC_DIR)/$(PREFIX)-size
GDB     = $(GCC_DIR)/$(PREFIX)-gdb

CFLAGS = -I $(SUPPORT_FILE_DIRECTORY) -mmcu=$(DEVICE) -Os -g
LFLAGS = -L $(SUPPORT_FILE_DIRECTORY) -mmcu=$(DEVICE) -g

$(ELF): $(OBJS)
	$(CC) $(LFLAGS) -o $@ $(OBJS)
	$(SIZE) $(ELF)

%.o: %.c
	$(CC) -c $(CFLAGS) $<
	$(CC) -MM $(CFLAGS) $< > $*.d

debug: $(ELF)
	$(GDB) $(ELF)

clean:
	rm -rf $(ELF) $(OBJS) *.d

flash: $(ELF)
	$(MSPDEBUG) rf2500 'erase' 'load $(PROJ).elf' 'exit'

-include $(OBJS:.o:.d)
