.include "config.mk"

WORKDIR=work
CFLAGS=-Wall -ansi -pedantic

.ifndef RELEASE
OUTDIR=debug
CFLAGS+=-O0 -g
.else
OUTDIR=dist
.endif

.ifdef DARWIN
LIBTIMELOGEXT=dylib
.else
LIBTIMELOGEXT=so
TIMELOGRELOC=-Wl,-z,origin,-rpath='$$ORIGIN/../lib/'
.endif

.PHONY: all
all: directories $(OUTDIR)/bin/tl

.PHONY: timelog_version.h
timelog_version.h:
	./version.sh

.PHONY: directories
directories: $(WORKDIR)/obj $(OUTDIR)/include $(OUTDIR)/lib $(OUTDIR)/bin

$(WORKDIR)/obj $(OUTDIR)/include $(OUTDIR)/lib $(OUTDIR)/bin:
	mkdir -p $@

.PHONY: clean
clean:
	rm -rf $(WORKDIR)

.PHONY: confclean
confclean: clean
	rm -f GNUmakefile
	rm -f conf.mk

.PHONY: distclean
distclean: clean
	rm -rf $(OUTDIR)

#
# libtimelog
#

$(OUTDIR)/lib/libtimelog.$(LIBTIMELOGEXT): $(WORKDIR)/obj/timelog.o
.ifdef DARWIN
	$(CC) -dynamiclib -Wl,-install_name,@loader_path/../lib/libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).dylib -o $(OUTDIR)/lib/libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINUSCLE timelog_version.h | cut -d' ' -f3).dylib $(WORKDIR)/obj/timelog.o -lcrypto
	ln -s libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINUSCLE timelog_version.h | cut -d' ' -f3).dylib $(OUTDIR)/lib/libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3).dylib
	ln -s libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3).dylib $(OUTDIR)/lib/libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).dylib
	ln -s libtimelog.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).dylib $@
.else
	$(CC) -shared -Wl,-soname,libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3) -o $(OUTDIR)/lib/libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINUSCLE timelog_version.h | cut -d' ' -f3) $(WORKDIR)/obj/timelog.o -lcrypto
	ln -s libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINUSCLE timelog_version.h | cut -d' ' -f3) $(OUTDIR)/lib/libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3)
	ln -s libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3).$$(grep TIMELOG_VERSION_MINOR timelog_version.h | cut -d' ' -f3) $(OUTDIR)/lib/libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3)
	ln -s libtimelog.so.$$(grep TIMELOG_VERSION_MAJOR timelog_version.h | cut -d' ' -f3) $@
.endif

$(WORKDIR)/obj/timelog.o: $(OUTDIR)/include/timelog.h
	$(CC) -I$(OUTDIR)/include -fPIC $(CFLAGS) -o $@ -c src/timelog.c

$(OUTDIR)/include/timelog.h: timelog_version.h src/include/timelog.h
	cat timelog_version.h src/include/timelog.h > $@

#
# tl
#

$(OUTDIR)/bin/tl: $(OUTDIR)/include/timelog.h $(OUTDIR)/lib/libtimelog.$(LIBTIMELOGEXT) src/tl.c
	$(CC) -I${OUTDIR}/include -L${OUTDIR}/lib $(TIMELOGRELOC) $(CFLAGS) \
	  -o $@ src/tl.c -ltimelog
