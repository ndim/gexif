AC_PREREQ(2.69)
AC_INIT([GTK EXIF viewer/editor],
        [0.5.0git3],
        [libexif-devel@lists.sourceforge.net],
        [gexif],
        [https://libexif.github.io/])
AC_CONFIG_SRCDIR([gexif/gexif.c])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([auto-m4])
AM_INIT_AUTOMAKE([
  gnu
  1.14.1
  dist-xz
  dist-bzip2
])
AM_SILENT_RULES([yes])

GP_CHECK_SHELL_ENVIRONMENT
GP_CONFIG_MSG([Build])
GP_CONFIG_MSG([Source code location], [${srcdir}])

AC_PROG_CC
AM_PROG_CC_STDC
AC_HEADER_STDC
LT_INIT([win32-dll])
AM_CPPFLAGS="$CPPFLAGS -I\$(top_srcdir)"


# ---------------------------------------------------------------------------
# i18n support
# ---------------------------------------------------------------------------
GP_GETTEXT_HACK([], [Lutz Mueller and others])
ALL_LINGUAS="de es fr pl sv"
AM_PO_SUBDIRS()
AM_GNU_GETTEXT_VERSION([0.18.3])
AM_GNU_GETTEXT([external])
AM_XGETTEXT_OPTION([--from-code=UTF-8])dnl macro since gettext 0.17
AM_ICONV()
GP_GETTEXT_FLAGS()


# ---------------------------------------------------------------------------
# conditional libraries
# ---------------------------------------------------------------------------
PKG_CHECK_MODULES([LIBEXIF_GTK2], [libexif-gtk >= 0.3.3],
                  [have_libexif_gtk2=yes], [have_libexif_gtk2=no])
PKG_CHECK_MODULES([LIBEXIF_GTK3], [libexif-gtk3 >= 0.3.3],
                  [have_libexif_gtk3=yes], [have_libexif_gtk3=no])

dnl Note that all dependent library flags for gtk+-2.0, gtk+-3.0,
dnl libexif, etc. will be pulled in indirectly LIBEXIF_GTK2_* and
dnl LIBEXIF_GTK3_*, so we do not need to explicitly check for these
dnl other libraries here.

AM_CONDITIONAL([HAVE_LIBEXIF_GTK2], [test "x$have_libexif_gtk2" = xyes])
AM_CONDITIONAL([HAVE_LIBEXIF_GTK3], [test "x$have_libexif_gtk3" = xyes])

AS_IF([test "x$have_libexif_gtk2" = xyes], [],
      [test "x$have_libexif_gtk3" = xyes], [],
      [AC_MSG_ERROR([pkg-config must find at least one of libexif-gtk, libexif-gtk3.])])


# ---------------------------------------------------------------------------
# Warnings: If we have GCC, be paranoid.
# ---------------------------------------------------------------------------
AS_VAR_IF([GCC], [yes], [dnl
    AM_CPPFLAGS="$AM_CPPFLAGS -Wall -Wchar-subscripts -Wmissing-declarations -Wmissing-prototypes -Wnested-externs -Wpointer-arith"
    AM_LDFLAGS="$AM_LDFLAGS -g -Wall"
])

AC_SUBST([AM_CPPFLAGS])dnl
AC_SUBST([AM_LDFLAGS])dnl


# ---------------------------------------------------------------------------
# Output files
# ---------------------------------------------------------------------------
AC_CONFIG_FILES([
  po/Makefile.in
  Makefile
  m4m/Makefile
  libjpeg/Makefile
  gexif/Makefile
])
AC_OUTPUT

GP_CONFIG_OUTPUT
