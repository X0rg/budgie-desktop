-include $(top_srcdir)/common.mk

EXTRA_DIST =
pkglib_LTLIBRARIES =
plugindir = $(MODULEDIR)
plugin_DATA =

%.plugin: %.plugin.in $(INTLTOOL_MERGE) $(wildcard $(top_srcdir)/po/*po)
	$(AM_V_GEN) \
	$(MKDIR_P) "$(dir $@)"; \
	$(INTLTOOL_MERGE) $(top_srcdir)/po $< $@ -d -u -c $(top_builddir)/po/.intltool-merge-cache

plugin_resources = $(shell glib-compile-resources --sourcedir=$(top_srcdir)/panel/applets/budgie-menu  --generate-dependencies $(top_srcdir)/panel/applets/budgie-menu/plugin.gresource.xml)

# resources
plugin-resources.h: $(top_srcdir)/panel/applets/budgie-menu/plugin.gresource.xml $(plugin_resources)
	glib-compile-resources --target=$@ --sourcedir=$(top_srcdir)/panel/applets/budgie-menu --generate-header --c-name budgie_menu $<

plugin-resources.c: $(top_srcdir)/panel/applets/budgie-menu/plugin.gresource.xml $(plugin_resources)
	glib-compile-resources --target=$@ --sourcedir=$(top_srcdir)/panel/applets/budgie-menu --generate-source --c-name budgie_menu $<

BUILT_SOURCES = \
	plugin-resources.h \
	plugin-resources.c

CLEANFILES = \
	$(BUILT_SOURCES) \
	BudgieMenu.plugin

DISTCLEANFILES = \
	BudgieMenu.plugin

	
plugin_DATA += \
	BudgieMenu.plugin

EXTRA_DIST += \
	BudgieMenu.plugin.in \
	com.solus-project.budgie-menu.gschema.xml \
	plugin.gresource.xml \
	settings.ui


plugin_LTLIBRARIES = libbudgiemenuapplet.la

libbudgiemenuapplet_la_SOURCES = \
	BudgieMenu.vala \
	BudgieMenuWindow.vala \
	plugin-resources.h \
	plugin-resources.c

libbudgiemenuapplet_la_CFLAGS = \
	$(BUDGIE_BASE_CFLAGS) \
	$(BUDGIE_PLUGIN_CFLAGS) \
	$(GMENU_CFLAGS) \
	-DGMENU_I_KNOW_THIS_IS_UNSTABLE \
	$(DECLARATIONS)

libbudgiemenuapplet_la_LIBADD = \
	${top_builddir}/plugin/libbudgie-plugin.la \
	$(BUDGIE_BASE_LIBS) \
	$(BUDGIE_PLUGIN_LIBS) \
	$(GMENU_LIBS)

libbudgiemenuapplet_la_LDFLAGS = \
	$(MODULE_FLAGS) \
	$(AM_LDFLAGS)

libbudgiemenuapplet_la_VALAFLAGS = \
	--pkg gtk+-3.0 \
	--pkg libgnome-menu-3.0 \
	--pkg gio-unix-2.0 \
	--pkg libpeas-1.0 \
	--pkg PeasGtk-1.0 \
	--pkg budgie-1.0 \
	--vapidir=${top_srcdir}/plugin \
	--target-glib=2.38 \
	--gresources=${srcdir}/plugin.gresource.xml

gsettings_SCHEMAS = \
	com.solus-project.budgie-menu.gschema.xml

@GSETTINGS_RULES@
