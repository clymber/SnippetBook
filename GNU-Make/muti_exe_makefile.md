```Makefile
# Makefile
#    compile every C language source file in into an executable

# Custom Command
GCC:= gcc
RM := rm -f

# Custom Options and Flags
OUTPUT_OPTION = -o $@
CFLAGS        += -Wall -DMACOS -D_DARWIN_C_SOURCE
LDFLAGS       +=

# Depended Library that Already Existed
LDLIBS := # -lgmock

# File lists
sources := $(wildcard *.c)
TARGET := $(subst .c,,${sources})

# Makefile targets
.PHONY: all
all: $(TARGET)

.PHONY: clean
clean:
	$(RM) $(TARGET)

# Makefile rules
%: %.c
	$(GCC) $(OUTPUT_OPTION) $(TARGET_ARCH) $^ $(LDFLAGS) $(LDLIBS) $(LOADLIBES)

```