#
# the DASM macro assembler (aka small systems cross assembler)
#
# Copyright (c) 1988-2002 by Matthew Dillon.
# Copyright (c) 1995 by Olaf "Rhialto" Seibert.
# Copyright (c) 2003-2008 by Andrew Davie.
# Copyright (c) 2008 by Peter H. Froehlich.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License along
# with this program; if not, write to the Free Software Foundation, Inc.,
# 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

# Executables will be generated in the trunk/src/ directory (the
# "current directory" as far as this Makefile is concerned). If
# you want to move them somewhere, trunk/bin/ say, do so in the
# trunk/ Makefile!


M32BIT = -m32

#GOPTI = -O -m32 # -O for more warnings; use -O0 for debugging, use -O3 for speed
GOPTI  = -O2 # -O for more warnings; use -O0 for debugging, use -O3 for speed
GOPTI += $(M32BIT)

#GWARN= -ansi -std=c18 -pedantic -Wall
GWARN =  -ansi -std=c99 -pedantic -Wall
GWARN += -Wmissing-declarations
GWARN += -Wmissing-prototypes
GWARN += -Wold-style-definition
GWARN += -Wshadow
GWARN += -Wcast-qual
GWARN += -Wnested-externs
GWARN += -Wunused 	# definition is weird in man gcc, effect is weird too
GWARN += -Wundef
GWARN += -Winline
GWARN += -Wfloat-equal
GWARN += -Wunreachable-code # seems to create 4 spurious warnings?
GWARN += -Wno-unused-parameter # no-unused tames extra a little
GWARN += -Wformat-y2k
GWARN += -Wformat-security
GWARN += -Wcast-align
GWARN += -Wpointer-arith
GWARN += -Wbad-function-cast
GWARN += -Wmisleading-indentation
#GWARN += -Wstrict-prototypes
#GWARN += -Wdeclaration-after-statement
#GWARN += -Wwrite-strings
#GWARN += -Wconversion # too many right now
#GWARN += -Wtraditional # too many, not sure if it's worth it anyway
#GWARN += -Wextra
#GWARN += -Wformat-nonliteral
#GWARN += -Wfour-char-constants # Apple OS X only option, not needed anyway?

GDEBG  = -g # -fprofile-arcs -ftest-coverage -pg # -D_FORTIFY_SOURCE=2
#GDEBG += -fprofile-arcs
GDEBG += -ftest-coverage
#GDEBG += -pg
#GDEBG += -D_FORTIFY_SOURCE=2

CFLAGS   = $(GOPTI) $(GWARN) $(GDEBG)
LDFLAGS  = -L/usr/lib32
LDFLAGS += $(M32BIT)

ifeq ($(OS),Windows_NT)
      detected_OS := Windows
else
      detected_OS := $(shell uname -s)
ifeq ($(detected_OS),Darwin)
      CFLAGS = -mmacosx-version-min=10.5
endif
endif

CC= gcc

OBJS= main.o ops.o globals.o exp.o symbols.o \
      mne6303.o mne6502.o mne65c02.o mne68705.o mne6811.o mnef8.o mne68908.o
SRCS= main.c ops.c globals.c exp.c symbols.c \
      mne6303.c mne6502.c mne65c02.c mne68705.c mne6811.c mnef8.c mne68908.c

ALL= dasm ftohex

all:  $(info    detected_OS is $(detected_OS)) \
      $(ALL)

dasm: $(OBJS)
	$(CC) $(CFLAGS) $(OBJS) -o dasm $(LDFLAGS)

ftohex: ftohex.o

obj: $(OBJS)

$(OBJS): asm.h

clean:
	rm -rf *.o $(ALL) \
	dasm-alpha-*.tar.gz \
	*.gcda *.gcno *.gcov gmon.out

# prepare an alpha release containing just the source code,
# nothing else is in the archive since it is not intended
# for the public, just designated volunteers

alpha:
	echo "This is an incomplete alpha release of DASM source code." >README.ALPHA
	echo "The purpose is to identify build problems, nothing more." >>README.ALPHA
	echo "Please do *not* re-distribute this release in any form!" >>README.ALPHA
	echo "Please do *not* distribute binaries derived from it either!" >>README.ALPHA
	tar zcvf dasm-alpha-`date +%F`.tar.gz README.ALPHA Makefile *.h *.c
	rm -rf README.ALPHA
