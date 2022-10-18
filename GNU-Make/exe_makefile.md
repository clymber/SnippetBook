# 生成可执行程序的 Makefile 模板
下面是用于将C/C++代码编译成可执行程序的Makefile模板

```Makefile
# Makefile
#    A simple makefile for compiling c/c++ source file in the same directory
#    into an executable

# Make Target
TARGET := test

# Custom Command
CC := g++
GCC:= gcc
RM := rm -f

# Custom Options and Flags
OUTPUT_OPTION = -o $@
CFLAGS        += -ansi -Wall -DMACOS -D_DARWIN_C_SOURCE
CPPFLAGS      +=
LDFLAGS       +=

# Depended Library that Already Existed
LDLIBS := -lgmock

# File lists
sources := $(wildcard *.c *.cpp)
objects := $(subst .c,.o,$(subst .cpp,.o,$(sources)))
depends = $(wildcard $(subst .o,.d,$(objects)))

# Makefile targets
.PHONY: all
all: $(TARGET)

$(TARGET): $(objects)
	$(CC) $(OUTPUT_OPTION) $(TARGET_ARCH) $^ $(LDFLAGS) $(LDLIBS) $(LOADLIBES)

.PHONY: clean
clean:
	$(RM) $(objects) $(depends)

.PHONY: distclean
distclean: clean
	$(RM) $(TARGET)

make_debug:
	# TARGET : $(TARGET)
	# CC: $(CC)
	# OUTPUT_OPTION: $(OUTPUT_OPTION)
	# LDFLAGS: $(LDFLAGS)
	# LDLIBS: $(LDLIBS)

$(eval $(if $(filter $(MAKECMDGOALS),clean distclean),,include $(depends)))

# Makefile rules
%.o: %.cpp
	$(CC) $(CPPFLAGS) $(TARGET_ARCH) -MM -MP -MT $@ -MF $(subst .o,.d,$@) $<
	$(CC) $(CPPFLAGS) $(CFLAGS) $(TARGET_ARCH) $(OUTPUT_OPTION) -c $<

%.o: %.c
	$(GCC) $(CPPFLAGS) $(TARGET_ARCH) -MM -MP -MT $@ -MF $(subst .o,.d,$@) $<
	$(GCC) $(CPPFLAGS) $(CFLAGS) $(TARGET_ARCH) $(OUTPUT_OPTION) -c $<
```
