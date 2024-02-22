# 生成可执行程序的 Makefile 模板
下面是用于将C/C++代码编译成可执行程序的Makefile模板

```Makefile
# Make Target
TARGET := test

# Custom Command
CC	:= gcc# C Complier Command
CXX	:= c++# C++ Complier command
RM	:= rm -f

# Custom Options and Flags
CFLAGS	+= -Wall# C compiler flags
CXXFLAGS += $(CFLAGS) -std=c++17# C++ compiler flags
CPPFLAGS += -I /usr/local/include# C/C++ preprocessor flags
LDFLAGS += -L /usr/local/lib# Linker flags

# Depended Library that Already Existed
LDLIBS := -lgmock -lgtest -lpthread

# File lists
sources := $(wildcard *.c *.cpp)
objects := $(subst .c,.o,$(subst .cpp,.o,$(sources)))
depends = $(wildcard $(subst .o,.d,$(objects)))

# Makefile targets
.PHONY: all clean distclean make_debug
all: $(TARGET)

clean:
	$(if $(wildcard $(objects))$(depends),$(RM) $(objects) $(depends),)

distclean: clean
	$(if $(wildcard $(TARGET)),$(RM) $(TARGET),)

make_debug:
	# Building targets  # TARGET: $(TARGET)
	# C complier        # CC: $(CC)
	# C compiler flags  # CFLAGS: $(CFLAGS)
	# C++ compiler      # CXX: $(CXX)
	# C++ compiler flags# CXXFLAGS: $(CXXFLAGS)
	# Linker options    # LDFLAGS: $(LDFLAGS)
	# Linking libraries # LDLIBS: $(LDLIBS)

# Include dependencies
$(eval $(if $(filter $(MAKECMDGOALS),clean distclean),,include $(depends)))

# Makefile rules
$(TARGET): $(objects)
	$(CXX) $^ $(LDFLAGS) $(LDLIBS) -o $@

%.o: %.cpp
	$(CXX) $< $(CPPFLAGS) -MM -MP -MT $@ -MF $(subst .o,.d,$@)
	$(CXX) $< $(CPPFLAGS) $(CXXFLAGS) -c -o $@

%.o: %.c
	$(CC) $< $(CPPFLAGS) -MM -MP -MT $@ -MF $(subst .o,.d,$@)
	$(CC) $< $(CPPFLAGS) $(CFLAGS) -c -o $@
```
