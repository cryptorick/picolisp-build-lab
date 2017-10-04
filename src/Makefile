# 04oct17abu
# (c) Software Lab. Alexander Burger

bin = ../bin
lib = ../lib

picoFiles = main.c gc.c apply.c flow.c sym.c subr.c big.c io.c net.c tab.c

CC = gcc

M32 = -m32

CFLAGS = -c -O2 -pipe \
	-falign-functions=32 -fomit-frame-pointer -fno-strict-aliasing \
	-W -Wimplicit -Wreturn-type -Wunused -Wformat \
	-Wuninitialized -Wstrict-prototypes \
	-D_GNU_SOURCE  -D_FILE_OFFSET_BITS=64

DYNAMIC-CC-FLAGS=

OS != uname | sed 's/Msys/Cygwin/'

Linux_CFLAGS = $(M32)
Linux_PICOLISP-FLAGS = $(M32) -rdynamic
Linux_LIB-FLAGS = -lm -ldl
Linux_DYNAMIC-LIB-FLAGS = $(M32) -shared -export-dynamic
Linux_LCRYPT = -lcrypt
Linux_STRIP = strip

OpenBSD_CFLAGS = $(M32)
OpenBSD_PICOLISP-FLAGS = $(M32) -rdynamic -Wl,-E
OpenBSD_LIB-FLAGS = -lm
OpenBSD_DYNAMIC-LIB-FLAGS = -Wl,-E -Wl,-shared
OpenBSD_LCRYPT = -lcrypto
OpenBSD_STRIP = strip

FreeBSD_CFLAGS = $(M32)
FreeBSD_PICOLISP-FLAGS = $(M32) -rdynamic
FreeBSD_LIB-FLAGS = -lm
FreeBSD_DYNAMIC-LIB-FLAGS = $(M32) -shared -export-dynamic
FreeBSD_LCRYPT = -lcrypt
FreeBSD_STRIP = strip

NetBSD_CFLAGS = $(M32)
NetBSD_PICOLISP-FLAGS = $(M32) -rdynamic
NetBSD_LIB-FLAGS = -lm
NetBSD_DYNAMIC-LIB-FLAGS = $(M32) -shared -export-dynamic
NetBSD_LCRYPT = -lcrypto
NetBSD_STRIP = strip

Darwin_CFLAGS = $(M32)
Darwin_PICOLISP-FLAGS = $(M32)
Darwin_LIB-FLAGS = -lc -lm -ldl
Darwin_DYNAMIC-LIB-FLAGS = $(M32) -dynamiclib -undefined dynamic_lookup
Darwin_LCRYPT = -lcrypto
Darwin_STRIP = strip -x

SunOS_CFLAGS = $(M32)
SunOS_PICOLISP-FLAGS =
SunOS_LIB-FLAGS = -lm -lnsl -lsocket
SunOS_DYNAMIC-LIB-FLAGS = -G
SunOS_LCRYPT = -lcrypt
SunOS_STRIP = strip

AIX_CFLAGS = -maix32
AIX_PICOLISP-FLAGS = -maix32 -Wl,-brtl -Wl,-bexpall
AIX_LIB-FLAGS = -lm
AIX_DYNAMIC-LIB-FLAGS = -maix32 -Wl,-G -shared -lm
AIX_LCRYPT = -lcrypt
AIX_STRIP = strip

HP-UX_CFLAGS =
HP-UX_PICOLISP-FLAGS = $(M32) -rdynamic
HP-UX_LIB-FLAGS = -lm -ldl
HP-UX_DYNAMIC-LIB-FLAGS = $(M32) -shared
HP-UX_LCRYPT =
HP-UX_STRIP = strip

IRIX64_CFLAGS = -std=gnu99
IRIX64_DYNAMIC-LIB-FLAGS = -shared
IRIX64_STRIP = strip -f

Cygwin_CFLAGS = $(M32)
Cygwin_DYNAMIC-LIB-FLAGS = -shared
Cygwin_PICOLISP-FLAGS =
Cygwin_DLL-DEFS = $(bin)/picolisp.dll
Cygwin_LCRYPT = -lcrypt
Cygwin_STRIP = strip
Cygwin_exe = .exe
Cygwin_dll = .dll

# CCLD is the cc (compiler frontend) to use for the link step.
# FreeBSD needs clang; all others use $(CC).
CCLD != test "$(OS)" == "FreeBSD" && echo "clang" || echo "$(CC)"

CFLAGS            := $(CFLAGS) $($(OS)_CFLAGS)
PICOLISP-FLAGS    := $($(OS)_PICOLISP-FLAGS)
LIB-FLAGS         := $($(OS)_LIB-FLAGS)
DYNAMIC-LIB-FLAGS := $($(OS)_DYNAMIC-LIB-FLAGS)
DLL-DEFS          := $($(OS)_DLL-DEFS)
LCRYPT            := $($(OS)_LCRYPT)
STRIP             := $($(OS)_STRIP)
exe               := $($(OS)_exe)
dll               := $($(OS)_dll)


picolisp: $(bin)/picolisp$(exe) $(lib)/ext$(dll) $(lib)/ht$(dll)
tools: $(bin)/lat1 $(bin)/utf2 $(bin)/balance
gate: $(bin)/ssl $(bin)/httpGate

all: picolisp tools gate

.c.o:
	$(CC) $(CFLAGS) -D_OS='"$(OS)"' $*.c

ext.o ht.o: pico.h
	$(CC) $(CFLAGS) -D_OS='"$(OS)"' $*.c $(DYNAMIC-CC-FLAGS)

$(picoFiles:.c=.o) ext.o ht.o: pico.h
main.o: vers.h


$(bin)/picolisp.dll: $(picoFiles:.c=.o)
	$(CCLD) -o $(bin)/picolisp.dll $(DYNAMIC-LIB-FLAGS) $(picoFiles:.c=.o)
	$(STRIP) $(bin)/picolisp.dll

$(bin)/picolisp.exe: $(picoFiles:.c=.o) $(bin)/picolisp.dll start.o
	mkdir -p $(bin) $(lib)
	$(CCLD) -o $(bin)/picolisp.exe $(PICOLISP-FLAGS) start.o -L$(bin) -l$(bin)/picolisp
	$(STRIP) $(bin)/picolisp.exe

$(bin)/picolisp: $(picoFiles:.c=.o)
	mkdir -p $(bin) $(lib)
	$(CCLD) -o $(bin)/picolisp$(exe) $(PICOLISP-FLAGS) $(picoFiles:.c=.o) $(LIB-FLAGS)
	$(STRIP) $(bin)/picolisp$(exe)


$(lib)/ext$(dll): ext.o
	$(CCLD) -o $(lib)/ext$(dll) $(DYNAMIC-LIB-FLAGS) ext.o $(DLL-DEFS) $(LCRYPT)
	$(STRIP) $(lib)/ext$(dll)

$(lib)/ht$(dll): ht.o
	$(CCLD) -o $(lib)/ht$(dll) $(DYNAMIC-LIB-FLAGS) ht.o $(DLL-DEFS)
	$(STRIP) $(lib)/ht$(dll)


$(bin)/lat1: lat1.c
	$(CC) -o $(bin)/lat1$(exe) lat1.c
	$(STRIP) $(bin)/lat1$(exe)

$(bin)/utf2: utf2.c
	$(CC) -o $(bin)/utf2$(exe) utf2.c
	$(STRIP) $(bin)/utf2$(exe)

$(bin)/balance: balance.c
	$(CC) -o $(bin)/balance$(exe) balance.c
	$(STRIP) $(bin)/balance$(exe)

$(bin)/ssl: ssl.c
	$(CC) -o $(bin)/ssl$(exe) ssl.c -lssl -lcrypto
	$(STRIP) $(bin)/ssl$(exe)

$(bin)/httpGate: httpGate.c
	$(CC) -o $(bin)/httpGate$(exe) httpGate.c -lssl -lcrypto
	$(STRIP) $(bin)/httpGate$(exe)


clean:
	rm -f *.o

debug:
	@echo "OS                = $(OS)"
	@echo "CC                = $(CC)"
	@echo "CCLD              = $(CCLD)"
	@echo "CFLAGS            = $(CFLAGS)"
	@echo "PICOLISP-FLAGS    = $(PICOLISP-FLAGS)"
	@echo "LIB-FLAGS         = $(LIB-FLAGS)"
	@echo "DYNAMIC-LIB-FLAGS = $(DYNAMIC-LIB-FLAGS)"
	@echo "LCRYPT            = $(LCRYPT)"
	@echo "DLL-DEFS          = $(DLL-DEFS)"
	@echo "STRIP             = $(STRIP)"
	@echo "exe               = $(exe)"
	@echo "dll               = $(dll)"

# vi:noet:ts=4:sw=4
