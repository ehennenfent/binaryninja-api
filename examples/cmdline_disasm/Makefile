# Path to prebuilt libbinaryninjaapi.a
BINJA_API_A := ../../bin/libbinaryninjaapi.a

# Path to binaryninjaapi.h and json
INC := -I../../

UNAME_S := $(shell uname -s)
ifeq ($(UNAME_S),Linux)
	# Path to binaryninja install
	BINJAPATH := $(HOME)/binaryninja/
	CC := gcc
else
	BINJAPATH := /Applications/Binary\ Ninja.app/Contents/MacOS
	CC := clang
endif

SRCDIR := src
BUILDDIR := build
TARGETDIR := bin

TARGETNAME := disasm
TARGET := $(TARGETDIR)/$(TARGETNAME)

SRCEXT := cpp
SOURCES := $(shell find $(SRCDIR) -type f -name *.$(SRCEXT))
OBJECTS := $(patsubst $(SRCDIR)/%,$(BUILDDIR)/%,$(SOURCES:.$(SRCEXT)=.o))

LIBS := -L $(BINJAPATH) -lbinaryninjacore
CPPFLAGS := -c -O2 -Wall -W -fPIC --std=c++11 -pipe

all: $(TARGET)

ifeq ($(UNAME_S),Linux)
$(TARGET): $(OBJECTS)
	@mkdir -p $(TARGETDIR)
	$(CC) $^ $(BINJA_API_A) $(LIBS) -Wl,-rpath=$(BINJAPATH) -ldl -o $@
else
$(TARGET): $(OBJECTS)
	@mkdir -p $(TARGETDIR)
	$(CC) $^ $(BINJA_API_A) $(LIBS) -o $@
	install_name_tool -change @rpath/libbinaryninjacore.dylib $(BINJAPATH)/libbinaryninjacore.dylib $@
endif

$(BUILDDIR)/%.o: $(SRCDIR)/%.$(SRCEXT)
	@mkdir -p $(BUILDDIR)
	$(CC) $(CPPFLAGS) $(INC) -c -o $@ $<

clean:
	$(RM) -r $(BUILDDIR) $(TARGETDIR)

.PHONY: clean
