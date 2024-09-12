ACLOCAL_AMFLAGS = -I m4 --install
AUTOMAKE_OPTIONS = foreign 1.10
DISTCLEANFILES = *~ autoscan.log
MAINTAINERCLEANFILES = \
	config.log config.status \
	$(srcdir)/Makefile.in \
	$(srcdir)/config.h.in $(srcdir)/config.h.in~ $(srcdir)/configure \
	$(srcdir)/install-sh $(srcdir)/ltmain.sh $(srcdir)/missing \
	$(srcdir)/depcomp $(srcdir)/aclocal.m4 $(srcdir)/ylwrap \
	$(srcdir)/config.guess $(srcdir)/config.sub
EXTRA_DIST = .gitignore

AM_CFLAGS = -std=gnu89 $(OPENSSL_CFLAGS) $(OPTIONAL_LIBCURL_CFLAGS)

bin_PROGRAMS = osslsigncode

osslsigncode_SOURCES = osslsigncode.c
osslsigncode_LDADD = $(OPENSSL_LIBS) $(OPTIONAL_LIBCURL_LIBS)

clean-local:
	-rm -rf *.dSYM || rmdir *.dSYM

distclean-local:
	-rm -rf autom4te.cache || rmdir autom4te.cache
