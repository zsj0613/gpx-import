# gpx-import/src/Makefile
#
# GPX Import tool for OpenStreetMap
#
# Copyright 2008 Daniel Silverstone <dsilvers@digital-scurf.org>
#

all: gpx-parse

DB := mysql

OPT := -O0
CFLAGS := $(OPT) -g -Wall -std=c99 -D_POSIX_SOURCE -D_BSD_SOURCE -D_XOPEN_SOURCE -include log.h

# Comment this out to get the DEBUG log lines
CFLAGS += -DNDEBUG

# Get dep files
CFLAGS += -MD

ifeq ($(DB),mysql)
CFLAGS += $(shell mysql_config --cflags)
LDFLAGS += $(shell mysql_config --libs)
endif

ifeq ($(DB),postgres)
CFLAGS += -I$(shell pg_config --includedir)
LDFLAGS += -L$(shell pg_config --libdir) -lpq
endif

MAINOBJS := main.o gpx.o log.o

ALLOBJS := $(MAINOBJS)

gpx-parse: $(ALLOBJS)
	gcc -o $@ $^ -lexpat -larchive -lz -lbz2 $(LDFLAGS)

clean:
	$(RM) *.o gpx-parse *.d

distclean: clean
	$(RM) *~

-include $(patsubst %.o,%.d,$(ALLOBJS))
