dnl#                                               -*- Autoconf -*-
dnl# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.60])dnl

AC_INIT([osslsigncode],[1.4],[mfive@users.sourceforge.net])
AC_CONFIG_AUX_DIR([.])
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([foreign 1.10])
AM_MAINTAINER_MODE

AC_CONFIG_SRCDIR([osslsigncode.c])
AC_CANONICAL_HOST

dnl# Checks for programs.
AC_PROG_CC
AC_PROG_CC_C_O
AC_USE_SYSTEM_EXTENSIONS

AC_ARG_ENABLE([strict],
	      [AS_HELP_STRING([--enable-strict],
                              [enable strict compile mode @<:@disabled@:>@])],
	      [],
	      [enable_strict="no"])dnl

AC_ARG_ENABLE([pedantic],
	      [AS_HELP_STRING([--enable-pedantic],
                          [enable pedantic compile mode @<:@disabled@:>@])],
	      [],
	      [enable_pedantic="no"])dnl

AC_ARG_WITH([curl],
	    [AS_HELP_STRING([--with-curl],[enable curl @<:@enabled@:>@])],
	    [],
	    [with_curl="yes"])dnl

if test "x${enable_pedantic}" = "xyes"; then
	enable_strict="yes";
	CFLAGS="${CFLAGS} -pedantic"
fi
if test "x${enable_strict}" = "xyes"; then
	CFLAGS="${CFLAGS} -Wall -Wextra"
fi

PKG_PROG_PKG_CONFIG
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MKDIR_P
AC_PROG_SED
AC_PROG_MAKE_SET

AC_C_CONST
AC_HEADER_STDC
AC_HEADER_TIME
AC_CHECK_HEADERS([sys/mman.h],
	         [],
	         [AC_MSG_ERROR([Need mmap to build.])])
AC_CHECK_FUNC([mmap],
	      [AC_DEFINE([HAVE_MMAP],[1],[Define to 1 if you have mmap])],
	      [AC_MSG_ERROR([Need mmap to build.])])dnl

AC_CHECK_LIB([dl],
	     [dlopen],
	     [DL_LIBS="-ldl"])dnl

PKG_CHECK_MODULES([OPENSSL],
	[libcrypto >= 0.9.8],
	[],
	[PKG_CHECK_MODULES([OPENSSL],
		[openssl >= 0.9.8],
		[],
		[AC_CHECK_LIB([crypto],
			[RSA_verify],
			[OPENSSL_LIBS="-lcrypto ${SOCKETS_LIBS} ${DL_LIBS}"],
			[AC_MSG_ERROR([OpenSSL 0.9.8 or later is required. http://www.openssl.org/])],
			[${DL_LIBS}])])])dnl

PKG_CHECK_MODULES([LIBCURL],
	          [libcurl >= 7.12.0],
	          [],
	          [AC_CHECK_LIB([curl],
		                [curl_easy_strerror],
		                [LIBCURL_LIBS="-lcurl"],
		                [],
		                [${DL_LIBS}])])dnl

if test "x${with_curl}" = "xyes"; then
	test -z "${LIBCURL_LIBS}" && AC_MSG_ERROR([Curl 7.12.0 or later is required for timestamping support. http://curl.haxx.se/])
	AC_CHECK_HEADERS([curl/curl.h])
	OPTIONAL_LIBCURL_CFLAGS="${LIBCURL_CFLAGS}"
	OPTIONAL_LIBCURL_LIBS="${LIBCURL_LIBS}"
	AC_DEFINE([ENABLE_CURL],[1],[libcurl is enabled])
fi

# Checks for header files.
AC_CHECK_HEADERS([ctype.h fcntl.h time.h])
AC_CHECK_HEADERS([openssl/err.h openssl/objects.h openssl/evp.h \
                  openssl/x509.h openssl/pkcs7.h openssl/pkcs12.h \
                  openssl/pem.h openssl/asn1t.h])dnl

if test "x${ac_cv_header_stdio_h}" = "x"; then
  test -z "${ac_cv_header_stdio_h}" || unset ac_cv_header_stdio_h
  AC_CHECK_HEADERS([stdio.h])
fi
if test "x${ac_cv_header_unistd_h}" = "x"; then
  test -z "${ac_cv_header_unistd_h}" || unset ac_cv_header_unistd_h
  AC_CHECK_HEADERS([unistd.h])
fi

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T

# Checks for library functions.
AC_FUNC_MMAP
AC_CHECK_FUNCS([memset])dnl

# Output.
AC_SUBST([OPTIONAL_LIBCURL_CFLAGS])dnl
AC_SUBST([OPTIONAL_LIBCURL_LIBS])dnl
AC_CONFIG_FILES([Makefile Makefile_orig])
AC_OUTPUT

