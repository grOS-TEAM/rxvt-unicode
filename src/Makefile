DATE=2010-12-13
VERSION=9.10
RXVTNAME=urxvt
SHELL = /bin/sh
prefix = /usr
exec_prefix = ${prefix}
bindir = ${exec_prefix}/bin
libdir = ${exec_prefix}/lib
includedir = ${prefix}/include
CC = gcc
CXX = g++
CPP = gcc -E
PERL = /usr/bin/perl
INSTALL = /usr/bin/install -c
INSTALL_PROGRAM = /usr/bin/install -c -m 755
INSTALL_DATA = /usr/bin/install -c -m 644
CXXFLAGS = -g -O3 -fno-rtti -fvisibility-inlines-hidden -fno-threadsafe-statics -fno-enforce-eh-specs -w
CPPFLAGS =   -I/usr/include/freetype2  -I/usr/include/freetype2 
LDFLAGS = 
DEFS = -DHAVE_CONFIG_H
LIBS = -lm  -lsupc++ -lm
XINC =   -pthread -I/usr/include/gdk-pixbuf-2.0 -I/usr/include/libpng12 -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include 
XLIB = -lfontconfig -lfreetype  -lXft  -lXrender -lX11    -lX11  -lgdk_pixbuf-2.0 -lgobject-2.0 -lglib-2.0 
COMPILE = $(CXX) -I.. -I$(srcdir) -I. -I$(srcdir)/../libev $(DEFS) $(CPPFLAGS) $(CXXFLAGS) $(XINC)
LINK = gcc $(LDFLAGS)
EXEEXT = 
PERLFLAGS =  -D_REENTRANT -D_GNU_SOURCE -DDEBIAN -fwrapv -fno-strict-aliasing -pipe -I/usr/local/include -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64  -I/usr/lib/x86_64-linux-gnu/perl/5.20/CORE 
PERLLIB = -Wl,-E  -fstack-protector -L/usr/local/lib  -L/usr/lib/x86_64-linux-gnu/perl/5.20/CORE -lperl -ldl -lm -lpthread -lc -lcrypt

srcdir =	.


first_rule: all
dummy:

COMMON = \
	background.o command.o rxvtfont.o init.o logging.o main.o misc.o \
	ptytty.o proxy.o screen.o scrollbar.o scrollbar-next.o scrollbar-rxvt.o \
	scrollbar-xterm.o scrollbar-plain.o xdefaults.o encoding.o \
	rxvttoolkit.o rxvtutil.o keyboard.o \
	fdpass.o ev_cpp.o rxvtperl.o

COMMON_DAEMON = rxvtdaemon.o

RXVT_BINNAME=$(DESTDIR)$(bindir)/$(RXVTNAME)$(EXEEXT)
RXVTC_BINNAME=$(DESTDIR)$(bindir)/$(RXVTNAME)c$(EXEEXT)
RXVTD_BINNAME=$(DESTDIR)$(bindir)/$(RXVTNAME)d$(EXEEXT)

#
# Distribution variables
#

.SUFFIXES:	.C .c .o

#-------------------------------------------------------------------------
# inference rules
.C.o:
	$(COMPILE) -c $<

#-------------------------------------------------------------------------

all: allbin

rxvt: rxvt.o $(COMMON)
	$(LINK) -o $@ rxvt.o $(COMMON) $(LIBS) $(XLIB) $(PERLLIB)

rxvtd: rxvtd.o $(COMMON) $(COMMON_DAEMON)
	$(LINK) -o $@ rxvtd.o $(COMMON) $(COMMON_DAEMON) $(LIBS) $(XLIB) $(PERLLIB)

rxvtc: rxvtc.o $(COMMON_DAEMON) fdpass.o
	$(LINK) -o $@ rxvtc.o $(COMMON_DAEMON) fdpass.o $(LIBS)

#-------------------------------------------------------------------------

#EXUBERANT_OPTS = -I NOTHROW

tags: *.[Ch]
	ctags $(EXUBERANT_OPTS) *.[Ch]

TAGS: *.[Ch]
	etags $(EXUBERANT_OPTS) *.[Ch]

allbin: rxvt rxvtd rxvtc

alldoc:

clean:
	rm -f rxvt rxvtc rxvtd perlxsi.c rxvtperl.C
	rm -f *.o

distclean: clean
	rm -f Makefile

realclean: distclean
	rm -f tags TAGS

cleandir: realclean

install-perl:
	$(INSTALL) -d $(DESTDIR)$(libdir)
	$(INSTALL) -d $(DESTDIR)$(libdir)/urxvt
	$(INSTALL) -d $(DESTDIR)$(libdir)/urxvt/perl
	$(INSTALL_DATA) $(srcdir)/urxvt.pm $(DESTDIR)$(libdir)/urxvt/urxvt.pm
	for ext in $(srcdir)/perl/*; do test -f "$$ext" && $(INSTALL_DATA) "$$ext" $(DESTDIR)$(libdir)/urxvt/perl/; done

install-bin: allbin
	$(INSTALL) -d $(DESTDIR)$(bindir)
	$(INSTALL_PROGRAM) rxvt  $(RXVT_BINNAME)
	$(INSTALL_PROGRAM) rxvtc $(RXVTC_BINNAME)
	$(INSTALL_PROGRAM) rxvtd $(RXVTD_BINNAME)

install: install-bin install-perl

perlxsi.c: Makefile
	$(PERL) -MExtUtils::Embed -e xsinit -- -std urxvt

rxvtperl.C: rxvtperl.xs iom_perl.h iom_perl.xs typemap typemap.iom
	PERL="$(PERL)" $(PERL) /usr/share/perl/5.20/ExtUtils/xsubpp -C++ -typemap /usr/share/perl/5.20/ExtUtils/typemap -typemap typemap.iom -typemap typemap -prototypes $(srcdir)/rxvtperl.xs >$@

rxvtperl.o: rxvtperl.C perlxsi.c
	$(COMPILE) $(PERLFLAGS) -DLIBDIR="\"$(libdir)/urxvt\"" -c $<

depend:
	makedepend -f Makefile.in -I. -I.. -I../libev -Y *.C >/dev/null 2>&1

# DO NOT DELETE: nice dependency list follows

background.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
background.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h
background.o: ../libev/ev.h callback.h scrollbar.h salloc.h libptytty.h
background.o: rxvtperl.h hookinc.h rsinc.h optinc.h
command.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
command.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
command.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
command.o: rsinc.h optinc.h version.h command.h keyboard.h
encoding.o: ../config.h encoding.h table/iso8859_1.h table/iso8859_15.h
encoding.o: table/iso8859_2.h table/iso8859_3.h table/iso8859_4.h
encoding.o: table/iso8859_5.h table/iso8859_6.h table/iso8859_7.h
encoding.o: table/iso8859_8.h table/iso8859_9.h table/iso8859_10.h
encoding.o: table/iso8859_11.h table/iso8859_13.h table/iso8859_14.h
encoding.o: table/iso8859_16.h table/koi8_r.h table/koi8_u.h
encoding.o: table/ksc5601_1987_0.h table/big5.h table/gbk_0.h
encoding.o: table/gb2312_1980_0.h table/cns11643_1992_1.h
encoding.o: table/cns11643_1992_2.h table/cns11643_1992_3.h
encoding.o: table/cns11643_1992_4.h table/cns11643_1992_5.h
encoding.o: table/cns11643_1992_6.h table/cns11643_1992_7.h
encoding.o: table/cns11643_1992_f.h table/big5_ext.h table/big5_plus.h
encoding.o: table/viscii.h table/jis0201_1976_0.h table/jis0208_1990_0.h
encoding.o: table/jis0212_1990_0.h table/jis0213_1.h table/jis0213_2.h
encoding.o: table/compose.h table/category.h
ev_cpp.o: ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h ../libev/ev.c
ev_cpp.o: ../libev/ev_vars.h ../libev/ev_epoll.c ../libev/ev_select.c
fdpass.o: ../config.h libptytty.h
init.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
init.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
init.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
init.o: rsinc.h optinc.h init.h keyboard.h
keyboard.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
keyboard.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
keyboard.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
keyboard.o: rsinc.h optinc.h keyboard.h command.h
logging.o: ../config.h ptytty.h libptytty.h ptytty_conf.h rxvt.h feature.h
logging.o: encoding.h rxvtutil.h rxvtfont.h rxvttoolkit.h ev_cpp.h
logging.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h salloc.h
logging.o: rxvtperl.h hookinc.h rsinc.h optinc.h
main.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
main.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
main.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
main.o: rsinc.h optinc.h init.h keyboard.h
misc.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
misc.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
misc.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
misc.o: rsinc.h optinc.h
proxy.o: ../config.h ptytty.h libptytty.h ptytty_conf.h rxvt.h feature.h
proxy.o: encoding.h rxvtutil.h rxvtfont.h rxvttoolkit.h ev_cpp.h
proxy.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h salloc.h
proxy.o: rxvtperl.h hookinc.h rsinc.h optinc.h
ptytty.o: ../config.h ptytty.h libptytty.h ptytty_conf.h rxvt.h feature.h
ptytty.o: encoding.h rxvtutil.h rxvtfont.h rxvttoolkit.h ev_cpp.h
ptytty.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h salloc.h
ptytty.o: rxvtperl.h hookinc.h rsinc.h optinc.h
rxvt.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
rxvt.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
rxvt.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
rxvt.o: rsinc.h optinc.h
rxvtc.o: ../config.h rxvtdaemon.h rxvtutil.h libptytty.h rxvt.h feature.h
rxvtc.o: encoding.h rxvtfont.h rxvttoolkit.h ev_cpp.h ../config.h
rxvtc.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h salloc.h
rxvtc.o: rxvtperl.h hookinc.h rsinc.h optinc.h
rxvtd.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
rxvtd.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
rxvtd.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
rxvtd.o: rsinc.h optinc.h rxvtdaemon.h
rxvtdaemon.o: rxvtdaemon.h rxvtutil.h
rxvtfont.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
rxvtfont.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
rxvtfont.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
rxvtfont.o: rsinc.h optinc.h table/linedraw.h
rxvtperl.o: ../config.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
rxvtperl.o: rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h rxvttoolkit.h
rxvtperl.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
rxvtperl.o: rsinc.h optinc.h keyboard.h perlxsi.c iom_perl.h
rxvttoolkit.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
rxvttoolkit.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h
rxvttoolkit.o: ../libev/ev.h callback.h scrollbar.h salloc.h libptytty.h
rxvttoolkit.o: rxvtperl.h hookinc.h rsinc.h optinc.h
rxvtutil.o: rxvtutil.h
salloc.o: salloc.h
screen.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
screen.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
screen.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
screen.o: rsinc.h optinc.h salloc.C
scrollbar-next.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h
scrollbar-next.o: rxvtfont.h rxvttoolkit.h ev_cpp.h ../config.h
scrollbar-next.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h
scrollbar-next.o: salloc.h libptytty.h rxvtperl.h hookinc.h
scrollbar-next.o: rsinc.h optinc.h
scrollbar-plain.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h
scrollbar-plain.o: rxvtfont.h rxvttoolkit.h ev_cpp.h ../config.h
scrollbar-plain.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h
scrollbar-plain.o: salloc.h libptytty.h rxvtperl.h hookinc.h
scrollbar-plain.o: rsinc.h optinc.h
scrollbar-rxvt.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h
scrollbar-rxvt.o: rxvtfont.h rxvttoolkit.h ev_cpp.h ../config.h
scrollbar-rxvt.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h
scrollbar-rxvt.o: salloc.h libptytty.h rxvtperl.h hookinc.h
scrollbar-rxvt.o: rsinc.h optinc.h
scrollbar-xterm.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h
scrollbar-xterm.o: rxvtfont.h rxvttoolkit.h ev_cpp.h ../config.h
scrollbar-xterm.o: ../libev/ev++.h ../libev/ev.h callback.h scrollbar.h
scrollbar-xterm.o: salloc.h libptytty.h rxvtperl.h hookinc.h
scrollbar-xterm.o: rsinc.h optinc.h
scrollbar.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
scrollbar.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
scrollbar.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
scrollbar.o: rsinc.h optinc.h
xdefaults.o: ../config.h rxvt.h feature.h encoding.h rxvtutil.h rxvtfont.h
xdefaults.o: rxvttoolkit.h ev_cpp.h ../config.h ../libev/ev++.h ../libev/ev.h
xdefaults.o: callback.h scrollbar.h salloc.h libptytty.h rxvtperl.h hookinc.h
xdefaults.o: rsinc.h optinc.h version.h keyboard.h
