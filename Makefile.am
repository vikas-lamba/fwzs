pkgdata_SCRIPTS = fwzsd fwzsapp
pkgdata_DATA = PolkitAuth.py

icondir = $(pkgdatadir)/icons
icon_DATA = firewall.png firewall_g.png firewall_x.png firewall_y.png

desktopdatadir = $(datadir)/applications
desktopdata_DATA = fwzsapp.desktop

dbusdatadir = @dbusdatadir@
dbussystemservicedir = $(dbusdatadir)/system-services
dbussystemservice_DATA = org.opensuse.zoneswitcher.service org.fedoraproject.FirewallD1.service

dbussystemconfigdir = $(sysconfdir)/dbus-1/system.d
dbussystemconfig_DATA = org.opensuse.zoneswitcher.conf

systemdservicedir= $(prefix)/lib/systemd/system
systemdservice_DATA = fwzsd.service

policydir = $(datadir)/PolicyKit/policy
policy_DATA = org.opensuse.zoneswitcher.policy

policy1dir = $(datadir)/polkit-1/actions
policy1_DATA = org.opensuse.zoneswitcher.policy

do_subst = sed -e 's,[@]sbindir[@],$(sbindir),' \
	   -e 's,[@]bindir[@],$(bindir),' \
	   -e 's,[@]icondir[@],$(icondir),'
do_subst_py = sed -e '/+++automake/{s@ICONDIR *= .*@ICONDIR = "$(icondir)"@;s@BINDIR *= .*@BINDIR = "$(bindir)"@}'

fwzsd: fwzsd.py Makefile
	@echo "creating $@"
	@$(do_subst_py) $(srcdir)/$< > $@
	@chmod +x $@

fwzsapp: fwzsapp.py Makefile
	@echo "creating $@"
	@$(do_subst_py) $(srcdir)/$< > $@
	@chmod +x $@

fwzsapp.desktop: fwzsapp.desktop.in Makefile
	@echo "creating $@"
	@$(do_subst) $(srcdir)/$< > $@

CLEANFILES = $(pkgdata_SCRIPTS) $(dbussystemservice_DATA) $(desktopdata_DATA)
EXTRA_DIST = fwzsd.py fwzsapp.py \
	     fwzsd.service.in \
	     org.opensuse.zoneswitcher.service \
	     org.fedoraproject.FirewallD1.service \
	     fwzsapp.desktop.in \
	     $(icon_DATA) \
	     $(pkgdata_DATA) \
	     org.opensuse.zoneswitcher.policy \
	     org.opensuse.zoneswitcher.conf

install-exec-local:
	$(INSTALL_SCRIPT) -d -m 755 $(DESTDIR)$(bindir) $(DESTDIR)$(sbindir)
	rm -f $(DESTDIR)$(bindir)/fwzsapp
	rm -f $(DESTDIR)$(sbindir)/fwzsd
	ln -s $(pkgdatadir)/fwzsapp $(DESTDIR)$(bindir)
	ln -s $(pkgdatadir)/fwzsd $(DESTDIR)$(sbindir)

install-data-local: install-policyDATA
	sed -i -e 's/auth_admin_keep/auth_admin_keep_always/g' $(DESTDIR)$(policydir)/org.opensuse.zoneswitcher.policy

uninstall-local:
	rm -f $(DESTDIR)$(bindir)/fwzsapp
	rm -f $(DESTDIR)$(sbindir)/fwzsd

SUBDIRS = po
