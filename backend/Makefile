#
# "$Id: Makefile 8400 2009-03-03 19:39:21Z mike $"
#
#   Backend makefile for the Common UNIX Printing System (CUPS).
#
#   Copyright 2007-2009 by Apple Inc.
#   Copyright 1997-2007 by Easy Software Products, all rights reserved.
#
#   These coded instructions, statements, and computer programs are the
#   property of Apple Inc. and are protected by Federal copyright
#   law.  Distribution and use rights are outlined in the file "LICENSE.txt"
#   which should have been included with this file.  If this file is
#   file is missing or damaged, see the license at "http://www.cups.org/".
#
#   This file is subject to the Apple OS-Developed Software exception.
#

include ../Makedefs

RBACKENDS =	ipp lpd $(DNSSD_BACKEND)
UBACKENDS =	$(PAP) $(LEGACY_BACKENDS) serial snmp socket usb
UNITTESTS =	test1284 testbackend testsupplies
TARGETS =	libbackend.a $(RBACKENDS) $(UBACKENDS)
LIBOBJS	=	ieee1284.o network.o runloop.o snmp-supplies.o
OBJS	=	ipp.o lpd.o dnssd.o pap.o parallel.o scsi.o serial.o snmp.o \
		socket.o test1284.o testbackend.o testsupplies.o usb.o


#
# Make all targets...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:


#
# Make unit tests...
#

unittests:	$(UNITTESTS)


#
# Clean all object files...
#

clean:
	$(RM) $(OBJS) $(TARGETS) $(UNITTESTS) $(LIBOBJS) http mdns


#
# Update dependencies (without system header dependencies...)
#

depend:
	makedepend -Y -I.. -fDependencies $(OBJS:.o=.c) >/dev/null 2>&1


#
# Install all targets...
#

install:	all install-data install-headers install-libs install-exec


#
# Install data files...
#

install-data:


#
# Install programs...
#

install-exec:
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/backend
	for file in $(RBACKENDS); do \
		$(LIBTOOL) $(INSTALL_BIN) -m 700 $$file $(SERVERBIN)/backend; \
	done
	for file in $(UBACKENDS); do \
		$(INSTALL_BIN) $$file $(SERVERBIN)/backend; \
	done
	$(RM) $(SERVERBIN)/backend/http
	$(LN) ipp $(SERVERBIN)/backend/http
	if test "x$(DNSSD_BACKEND)" != x; then \
		$(RM) $(SERVERBIN)/backend/mdns; \
		$(LN) $(DNSSD_BACKEND) $(SERVERBIN)/backend/mdns; \
	fi
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(TARGETS); do \
			cp $$file $(SYMROOT); \
		done \
	fi


#
# Install headers...
#

install-headers:


#
# Install libraries...
#

install-libs:


#
# Uninstall all targets...
#

uninstall:
	for file in $(RBACKENDS) $(UBACKENDS); do \
		$(RM) $(SERVERBIN)/backend/$$file; \
	done
	$(RM) $(SERVERBIN)/backend/http
	-$(RMDIR) $(SERVERBIN)/backend
	-$(RMDIR) $(SERVERBIN)


#
# test1284
#

test1284:	test1284.o ../cups/libcups.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o test1284 test1284.o ../cups/libcups.a \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)


#
# testbackend
#

testbackend:	testbackend.o ../cups/libcups.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o testbackend testbackend.o ../cups/libcups.a \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)


#
# testsupplies
#

testsupplies:	testsupplies.o libbackend.a ../cups/libcups.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o testsupplies testsupplies.o libbackend.a \
		../cups/libcups.a $(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) \
		$(COMMONLIBS) $(LIBZ)


#
# libbackend.a
#

libbackend.a:	$(LIBOBJS)
	echo Archiving $@...
	$(RM) $@
	$(AR) $(ARFLAGS) $@ $(LIBOBJS)
	$(RANLIB) $@


#
# dnssd
#

dnssd:	dnssd.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o dnssd dnssd.o libbackend.a $(LIBS)
	$(RM) mdns
	$(LN) dnssd mdns


#
# ipp
#

ipp:	ipp.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o ipp ipp.o libbackend.a $(LIBS)
	$(RM) http
	$(LN) ipp http


#
# lpd
#

lpd:	lpd.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o lpd lpd.o libbackend.a $(LIBS)


#
# pap
#

pap:	pap.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o pap pap.o $(BACKLIBS) $(LIBS) -framework AppleTalk


#
# parallel
#

parallel:	parallel.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o parallel parallel.o libbackend.a $(LIBS)


#
# scsi
#

scsi:	scsi.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o scsi scsi.o $(LIBS)

scsi.o:	scsi.c scsi-irix.c scsi-linux.c


#
# serial
#

serial:	serial.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o serial serial.o libbackend.a $(BACKLIBS) $(LIBS)


#
# snmp
#

snmp:	snmp.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o snmp snmp.o libbackend.a $(LIBS)


#
# socket
#

socket:	socket.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o socket socket.o libbackend.a $(LIBS)


#
# usb
#

usb:	usb.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o usb usb.o libbackend.a $(LIBUSB) \
		$(BACKLIBS) $(LIBS)
usb.o:	usb.c usb-darwin.c usb-libusb.c usb-unix.c


#
# Dependencies...
#

include Dependencies


#
# End of "$Id: Makefile 8400 2009-03-03 19:39:21Z mike $".
#
