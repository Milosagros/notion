##
## Notion manual page Makefile
##

# System-specific configuration is in system.mk
TOPDIR=..
include $(TOPDIR)/build/system-inc.mk

######################################

SHELL=bash -o pipefail

TARGETS=notion.1 notionflux.1 welcome.txt

MKMAN=$(LUA) ../build/mkman.lua
NROFF=nroff -man -Tlatin1
#FILTERCRAP=perl -p -i -e 's/.\10//g'
FILTERCRAP=$(LUA) -e 'io.write((string.gsub(io.read("*a"), ".\8", "")))'
LUA_REFVERSION:=$(shell $(LUA) -e 'print(_VERSION:match("[\x25d.]+"))')

CONFIGS=../etc/cfg_bindings.lua \
	../etc/cfg_tiling.lua \
	../etc/cfg_query.lua \
	../etc/cfg_menu.lua

######################################

include $(TOPDIR)/build/rules.mk

######################################

notion.1: notion.in $(CONFIGS)
	$(MKMAN) -i $< -o $@ -D ETCDIR $(ETCDIR) -D DOCDIR $(DOCDIR) $(CONFIGS)

notion.%.1: notion.%.in $(CONFIGS) ../po/%.po
	$(MKMAN) -po ../po/$*.po -i $< -o $@ -D ETCDIR $(ETCDIR) -D DOCDIR $(DOCDIR) $(CONFIGS)

notionflux.1: notionflux.in $(CONFIGS)
	$(MKMAN) -i $< -o $@ -D DOCDIR $(DOCDIR) -D LUA_REFVERSION $(LUA_REFVERSION) $(CONFIGS)

welcome%txt: welcome%head notion%1
	(cat welcome$*head; \
	$(NROFF) notion$*1 | $(FILTERCRAP)) > $@

_install:
	$(INSTALLDIR) $(DESTDIR)$(MANDIR)/man1
	$(INSTALL) -m $(DATA_MODE) notion.1 $(DESTDIR)$(MANDIR)/man1
	$(INSTALL) -m $(DATA_MODE) notionflux.1 $(DESTDIR)$(MANDIR)/man1
	$(INSTALLDIR) $(DESTDIR)$(SHAREDIR)
	for i in $(TARGETS); do \
		$(INSTALL) -m $(DATA_MODE) $$i $(DESTDIR)$(SHAREDIR) ; \
	done

clean:
	$(RM) -f $(TARGETS)

.PHONY: clean
