## Process this file with automake to produce Makefile.in
ACLOCAL_AMFLAGS = -I m4 ${ACLOCAL_FLAGS}

SUBDIRS = pixmaps po scripts src help

uidir = $(pkgdatadir)
ui_DATA = data/preferences.ui \
          data/renice.ui \
          data/lsof.ui \
          data/openfiles.ui \
          data/interface.ui \
          data/menus.ui \
          data/popups.ui


runnabledir = $(pkglibexecdir)
runnable_SCRIPTS = scripts/gsm-renice \
                   scripts/gsm-kill

EXTRA_DIST = \
	$(ui_DATA) \
	$(runnable_SCRIPTS) \
	org.gnome.gnome-system-monitor.policy.in \
	gnome-system-monitor.desktop.in \
	gnome-system-monitor.doap \
	intltool-extract.in \
	intltool-merge.in \
	intltool-update.in

org.gnome.gnome-system-monitor.policy.in: org.gnome.gnome-system-monitor.policy.in.in Makefile
	$(AM_V_GEN) sed -e "s|\@pkglibexecdir\@|$(pkglibexecdir)|" $< > $@

@INTLTOOL_POLICY_RULE@
polkit_policydir = $(datadir)/polkit-1/actions
polkit_policy_in_files = org.gnome.gnome-system-monitor.policy.in
polkit_policy_DATA = $(polkit_policy_in_files:.policy.in=.policy)

Applicationsdir = $(datadir)/applications
Applications_in_files = gnome-system-monitor.desktop.in
Applications_DATA = $(Applications_in_files:.desktop.in=.desktop)
@INTLTOOL_DESKTOP_RULE@

distuninstallcheck_listfiles = find . -type f -print | grep -v 'scrollkeeper'

if MAINTAINER_MODE
dist-hook: dist-changelog
else
dist-hook:
endif

.PHONY: dist-changelog

dist-changelog:
	$(AM_V_at)if git --git-dir=$(top_srcdir)/.git --work-tree=$(top_srcdir) \
		log --no-merges --date=short --pretty='tformat:%cd  %an  <%ae>%n%n%s%n%n%b' a5b4b868024f89e07fb9e1bf35d94df60568d841.. | \
	 $(SED)	-e '/^[12]...-[01].-[0123].  [^<>]*  <[^<>]*>$$/,/^$$/ b' \
		-e '/[^	 ]/,/^[	 ]*$$/ !d' \
		-e 's/^[	 ]*/	/' \
		-e 's/^[	 ]*$$//' >.ChangeLog.tmp; \
	then mv -f .ChangeLog.tmp "$(top_distdir)/ChangeLog"; \
	else rm -f .ChangeLog.tmp; exit 1; fi

dist_noinst_DATA = \
	ChangeLog.pre-git

DISTCLEANFILES = \
	intltool-extract              \
	intltool-merge                \
	intltool-update               \
	gnome-system-monitor.desktop  \
	org.gnome.gnome-system-monitor.policy \
        org.gnome.gnome-system-monitor.policy.in 

DISTCHECK_CONFIGURE_FLAGS = --disable-scrollkeeper
