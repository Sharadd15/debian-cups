#
# "$Id: Makefile 8431 2009-03-12 22:59:58Z mike $"
#
#   Locale file makefile for the Common UNIX Printing System (CUPS).
#
#   Copyright 2007-2008 by Apple Inc.
#   Copyright 1993-2007 by Easy Software Products.
#
#   These coded instructions, statements, and computer programs are the
#   property of Apple Inc. and are protected by Federal copyright
#   law.  Distribution and use rights are outlined in the file "LICENSE.txt"
#   which should have been included with this file.  If this file is
#   file is missing or damaged, see the license at "http://www.cups.org/".
#

include ../Makedefs


OBJS	=	checkpo.o po2strings.o strings2po.o translate.o
TARGETS	=	checkpo po2strings strings2po translate


#
# Make everything...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:


#
# Make unit tests...
#

unittests:


#
# Clean all config and object files...
#

clean:
	$(RM) $(TARGETS) $(OBJS)


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

install-data: $(INSTALL_LANGUAGES)

install-languages:
	$(INSTALL_DIR) -m 755 $(LOCALEDIR)
	for loc in $(LANGUAGES) ; do \
		if test -f cups_$$loc.po; then \
			$(INSTALL_DIR) -m 755 $(LOCALEDIR)/$$loc ; \
			$(INSTALL_DATA) cups_$$loc.po $(LOCALEDIR)/$$loc/cups_$$loc.po ; \
		fi ; \
	done


#
# Install programs...
#

install-exec:


#
# Install headers...
#

install-headers:


#
# Install libraries...
#

install-libs:


#
# Uninstall files...
#

uninstall: $(UNINSTALL_LANGUAGES)

uninstall-languages:
	-for loc in $(LANGUAGES) ; do \
		$(RM) $(LOCALEDIR)/$$loc/cups_$$loc.po ; \
	done


#
# pot - Creates/updates the cups.pot template file, and merges changes
#       into existing message catalogs.
#

pot:
	echo Updating cups.pot...
	mv cups.pot cups.pot.bck
	touch cups.pot
	cd ..; xgettext -o locale/cups.pot -j -s --keyword=_ --no-wrap \
		--copyright-holder="Apple Inc." \
		--msgid-bugs-address="http://www.cups.org/str.php" \
		*/*.c */*.cxx
	(cat cups.header; \
	 cat cups.pot | sed -e '1,6d' -e '1,$$s/PACKAGE VERSION/CUPS 1.4/' \
	     -e '1,$$s/charset=CHARSET/charset=utf-8/'; \
	 cat cups.footer) > cups.pot.N
	mv cups.pot.N cups.pot
	for loc in $(LANGUAGES) ; do \
		echo Merging changes into cups_$$loc.po... ; \
		msgmerge -o cups_$$loc.po -s -N --no-location cups_$$loc.po cups.pot ; \
	done


#
# checkpo - A simple utility to check PO files for correct translation
#           strings.  Dependency on static library is deliberate.
#
# checkpo filename.po [... filenameN.po]
#

checkpo:	checkpo.o ../cups/libcups.a
	echo Linking $<...
	$(CC) $(LDFLAGS) -o checkpo checkpo.o ../cups/libcups.a \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)

checkall:	checkpo
	for file in *.po; do \
		./checkpo $$file; \
	done


#
# po2strings - A simple utility which uses iconv to convert GNU gettext
#              message catalogs to Mac OS X .strings files.
#
# po2strings filename.po filename.strings
#

po2strings:	po2strings.o ../cups/libcups.a
	echo Linking $<...
	$(CC) $(LDFLAGS) -o po2strings po2strings.o ../cups/libcups.a \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)


#
# strings2po - A simple utility which uses iconv to convert Mac OS X
#              .strings files to GNU gettext message catalogs.
#
# strings2po filename.strings filename.po
#

strings2po:	strings2po.o
	echo Linking $<...
	$(CC) $(LDFLAGS) -o strings2po strings2po.o


#
# translate - A simple utility which uses Google to translate the cups.pot
#             file to one of several languages.
#
# translate outfile language
#

translate:	translate.o ../cups/libcups.a
	echo Linking $<...
	$(CC) $(LDFLAGS) -o translate translate.o ../cups/libcups.a \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)


#
# Dependencies...
#
  
include Dependencies


#
# End of "$Id: Makefile 8431 2009-03-12 22:59:58Z mike $".
#
