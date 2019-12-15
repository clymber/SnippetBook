# 编译静态库的Makefile模板
```makefile
# a Makefile for compiling static library

TARGET    = libapue.so
CC        = gcc
CFLAGS    += -ansi -Wall -DMACOS -D_DARWIN_C_SOURCE
CPPFLAGS  +=
AR        = ar
ARFLAGS   = -rc
RM        = rm
RMFLAGS   = -f
TARGET_ARCH =
OUTPUT_OPTION= -o $@

# file lists
sources = $(wildcard *.c)
objects = $(patsubst %.c,%.o,$(patsubst %.cpp,%.o,$(sources)))
depends = $(wildcard $(patsubst %.o,%.d,$(objects)))

# Makefile command goals
.PHONY: all clean distclean makedebug

# default command goal
all: $(TARGET)

# Makefile command goal: TARGET
$(TARGET): $(objects)
	$(AR) $(ARFLAGS) $@ $?

clean:
	$(if $(wildcard $(objects)),$(RM) $(RMFLAGS) $(objects),)
	$(if $(wildcard $(depends)),$(RM) $(RMFLAGS) $(depends),)

distclean: clean
	$(if $(wildcard $(TARGET)),$(RM) $(RMFLAGS) $(TARGET),)

# dependencies
$(eval $(if $(filter $(MAKECMDGOALS),clean distclean makedebug),,include $(depends)))

# compile rules
%.o: %.c
	$(CC) $(CPPFLAGS) $(TARGET_ARCH) -MM -MP -MT $@ -MF $(subst .o,.d,$@) $<
	$(CC) $(CPPFLAGS) $(CFLAGS) $(TARGET_ARCH) $(OUTPUT_OPTION) -c $<

# debug this Makefile itself
makedebug:
	#sources	: $(sources)
	#objects	: $(objects)
	#depends	: $(depends)
```
