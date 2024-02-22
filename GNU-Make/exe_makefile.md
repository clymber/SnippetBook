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
OUTPUT = -o $@
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
.PHONY: all
all: $(TARGET)

$(TARGET): $(objects)
	$(CXX) $(LDFLAGS) $(LOADLIBES) $^ $(LDLIBS) $(OUTPUT)

.PHONY: clean
clean:
	$(RM) $(objects) $(depends)

.PHONY: distclean
distclean: clean
	$(RM) $(TARGET)

make_debug:
	# TARGET : $(TARGET)
	# CXX: $(CXX)
	# OUTPUT: $(OUTPUT)
	# LDFLAGS: $(LDFLAGS)
	# LDLIBS: $(LDLIBS)

$(eval $(if $(filter $(MAKECMDGOALS),clean distclean),,include $(depends)))

# Makefile rules
%.o: %.cpp
	$(CXX) $(CPPFLAGS) -MM -MP -MT $@ $< -MF $(subst .o,.d,$@)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $< $(OUTPUT)

%.o: %.c
	$(CC) $(CPPFLAGS) -MM -MP -MT $@ $< -MF $(subst .o,.d,$@)
	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< $(OUTPUT)
```
