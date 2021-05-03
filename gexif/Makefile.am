bin_PROGRAMS =
CLEANFILES =
EXTRA_PROGRAMS =
noinst_PROGRAMS =


########################################################################
# dependency hack for recursive make
########################################################################

gexif_ldadd        += $(top_builddir)/libjpeg/libjpeg.la
gexif_dependencies += $(top_builddir)/libjpeg/libjpeg.la-stamp
$(top_builddir)/libjpeg/libjpeg.la-stamp:
	cd $(top_builddir)/libjpeg && $(MAKE) libjpeg.la


########################################################################
# Definitions common to gexif-gtk*
########################################################################

gexif_sources =
gexif_sources += gexif.c
gexif_sources += gexif-main.c
gexif_sources += gexif-main.h
gexif_sources += gexif-thumbnail.c
gexif_sources += gexif-thumbnail.h

gexif_cppflags =
gexif_cppflags += $(AM_CPPFLAGS)
gexif_cppflags += $(CPPFLAGS)

gexif_ldadd =

gexif_dependencies =


########################################################################
# Build the gtk2 variant as gexif-gtk2
########################################################################

if HAVE_LIBEXIF_GTK2
noinst_PROGRAMS += gexif-gtk2
endif
EXTRA_PROGRAMS  += gexif-gtk2

gexif_gtk2_SOURCES      = $(gexif_sources)
gexif_gtk2_CPPFLAGS     = $(gexif_cppflags)
gexif_gtk2_LDADD        = $(gexif_ldadd)
gexif_gtk2_DEPENDENCIES = $(gexif_dependencies)

gexif_gtk2_CPPFLAGS += $(LIBEXIF_GTK2_CFLAGS)
gexif_gtk2_LDADD    += $(LIBEXIF_GTK2_LIBS)


########################################################################
# Build the gtk3 variant as gexif-gtk3
########################################################################

if HAVE_LIBEXIF_GTK3
noinst_PROGRAMS += gexif-gtk3
endif
EXTRA_PROGRAMS  += gexif-gtk3

gexif_gtk3_SOURCES      = $(gexif_sources)
gexif_gtk3_CPPFLAGS     = $(gexif_cppflags)
gexif_gtk3_LDADD        = $(gexif_ldadd)
gexif_gtk3_DEPENDENCIES = $(gexif_dependencies)

gexif_gtk3_CPPFLAGS += $(LIBEXIF_GTK3_CFLAGS)
gexif_gtk3_LDADD    += $(LIBEXIF_GTK3_LIBS)


########################################################################
# Install gexif executable to $(bindir)
########################################################################

# We let the autotools install the newer of the two executables, as
# this might include non-trivial libtool activities like re-linking.
# After that, the hook rule can run "mv -f" to rename the properly
# installed executable to the desired name gexif$(EXEEXT).

if HAVE_LIBEXIF_GTK3
the_gexif     = gexif-gtk3$(EXEEXT)
bin_PROGRAMS += gexif-gtk3
else
if HAVE_LIBEXIF_GTK2
the_gexif     = gexif-gtk2$(EXEEXT)
bin_PROGRAMS += gexif-gtk2
endif
endif

install-exec-hook:
	cd $(DESTDIR)$(bindir) && mv -f $(the_gexif) gexif$(EXEEXT)

uninstall-hook:
	cd $(DESTDIR)$(bindir) && rm -f gexif$(EXEEXT)
