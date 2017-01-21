#
# xAutoClick
#
# Copyright (C) 2006, 2010 Ivo van Poorten
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
#

include config.mak

OBJScommon	= main.o \
			  osdep.o \

PRGgtk2		= gautoclick2
OBJSgtk2	= $(OBJScommon) \
			  gtk2.o \

PRGascii	= aautoclick
OBJSascii	= $(OBJScommon) \
			  ascii.o \

PRGqt4		= qt4autoclick
OBJSqt4		= $(OBJScommon) \
			  qt4.o \
			  qt4-moc.o \

PRGqt5		= qt5autoclick
OBJSqt5		= $(OBJScommon) \
			  qt5.o \
			  qt5-moc.o \

PRGqt5		= qt5autoclick
OBJSqt5		= $(OBJScommon) \
			  guiqt5.o \
			  guiqt5-moc.o \

PRGcmdl		= cautoclick
OBJScmdl	= $(OBJScommon) \
			  commandline.o \

ifeq ($(__BUILD_GTK2__),yes)
INCLUDES	+= $(INCLUDES_GTK2)
LDFLAGS		+= $(LDFLAGS_GTK2)
endif

ifeq ($(__BUILD_ASCII__),yes)
INCLUDES	+= $(INCLUDES_ASCII)
LDFLAGS		+= $(LDFLAGS_ASCII)
endif

ifeq ($(__BUILD_QT4__),yes)
INCLUDES	+= $(INCLUDES_QT4)
LDFLAGS		+= $(LDFLAGS_QT4)
endif

ifeq ($(__BUILD_QT5__),yes)
INCLUDES	+= $(INCLUDES_QT5)
LDFLAGS		+= $(LDFLAGS_QT5) -fPIC
endif

ifeq ($(__BUILD_COMMANDLINE__),yes)
INCLUDES	+= $(INCLUDES_COMMANDLINE)
LDFLAGS		+= $(LDFLAGS_COMMANDLINE)
endif

.PHONY:	all
all:
ifeq ($(HAVE_GTK2),yes)
	__BUILD_GTK2__=yes $(MAKE) -C . $(PRGgtk2)
endif
ifeq ($(HAVE_ASCII),yes)
	__BUILD_ASCII__=yes $(MAKE) -C . $(PRGascii)
endif
ifeq ($(HAVE_QT4),yes)
	__BUILD_QT4__=yes $(MAKE) -C . $(PRGqt4)
endif
ifeq ($(HAVE_QT5),yes)
	__BUILD_QT5__=yes $(MAKE) -C . $(PRGqt5)
endif
ifeq ($(HAVE_COMMANDLINE),yes)
	__BUILD_COMMANDLINE__=yes $(MAKE) -C . $(PRGcmdl)
endif

$(PRGgtk2):		$(OBJSgtk2)
	$(CC) -o $@ $(OBJSgtk2) $(LDFLAGS)

$(PRGascii):	$(OBJSascii)
	$(CC) -o $@ $(OBJSascii) $(LDFLAGS)

$(PRGqt4):		$(OBJSqt4)
	$(CXX) -o $@ $(OBJSqt4) $(LDFLAGS)

$(PRGqt5):		$(OBJSqt5)
	$(CXX) -o $@ $(OBJSqt5) $(LDFLAGS)

$(PRGcmdl):		$(OBJScmdl)
	$(CC) -o $@ $(OBJScmdl) $(LDFLAGS)

qt4-moc.cpp:	qt4.h
	moc-qt4 -oqt4-moc.cpp qt4.h

qt5-moc.cpp:	qt5.h
	moc-qt5 -oqt5-moc.cpp qt5.h

guiqt5-moc.cpp:	guiqt5.h
	moc-qt5 -oguiqt5-moc.cpp guiqt5.h

#strip:	$(PRG)
#	strip $(PRG)

config.mak:		configure
	@echo
	@echo "#############################"
	@echo "Please run ./configure first!"
	@echo "#############################"
	@echo
	@echo "If you have done so before, you have to do it again, because it"
	@echo "has changed."
	@echo
	@exit 1

ifneq ($(OBJSgtk2),)
-include $(patsubst %,.deps/%,$(OBJSgtk2:.o=.d))
endif

ifneq ($(OBJSascii),)
-include $(patsubst %,.deps/%,$(OBJSascii:.o=.d))
endif

ifneq ($(OBJSqt4),)
-include $(patsubst %,.deps/%,$(OBJSqt4:.o=.d))
endif

ifneq ($(OBJSqt5),)
-include $(patsubst %,.deps/%,$(OBJSqt5:.o=.d))
endif

SILENCE = @

%.o:	%.c
	$(CC) -c $(CFLAGS) $(INCLUDES) $*.c -o $*.o $(DEBUG) $(WARNINGS)
	mkdir -p .deps
	$(CCFORDEPS) $(CFLAGS) $(INCLUDES) $*.c 1>.deps/$*.d

%.o:	%.cpp
	$(CXX) -c $(CFLAGS) -fPIC $(INCLUDES) $*.cpp -o $*.o $(DEBUG) $(WARNINGS)
	mkdir -p .deps
	$(CXXFORDEPS) $(CFLAGS) -fPIC $(INCLUDES) $*.cpp 1>.deps/$*.d

.PHONY:	clean
clean:
	rm -f *.o $(PRGgtk2) $(PRGascii) $(PRGcmdl)
	rm -f qt4-moc.cpp $(PRGqt4)
	rm -f qt5-moc.cpp $(PRGqt5)
	rm -rf .deps

.PHONY:	distclean
distclean:		clean
	rm -f config.mak configure.log
	rm -f *~

.PHONY:	install
install:		all
	@echo Installing to $(PREFIX)
	if test ! -d $(PREFIX)/bin ; then mkdir -p $(PREFIX)/bin ; fi
	if test ! -d $(PREFIX)/man/man1 ; then mkdir -p $(PREFIX)/man/man1 ; fi
ifeq ($(HAVE_GTK2),yes)
	cp $(PRGgtk2) $(PREFIX)/bin
endif
ifeq ($(HAVE_QT4),yes)
	cp $(PRGqt4) $(PREFIX)/bin
endif
ifeq ($(HAVE_QT5),yes)
	cp $(PRGqt5) $(PREFIX)/bin
endif
ifeq ($(HAVE_ASCII),yes)
	cp $(PRGascii) $(PREFIX)/bin
endif
ifeq ($(HAVE_COMMANDLINE),yes)
	cp $(PRGcmdl) $(PREFIX)/bin
endif
	cp xautoclick.1 $(PREFIX)/man/man1

.PHONY: uninstall
uninstall:
	@echo Uninstalling from $(PREFIX)
	rm -f $(PREFIX)/bin/$(PRGgtk2)
	rm -f $(PREFIX)/bin/$(PRGqt4)
	rm -f $(PREFIX)/bin/$(PRGqt5)
	rm -f $(PREFIX)/bin/$(PRGascii)
	rm -f $(PREFIX)/bin/$(PRGcmdl)
	rm -f $(PREFIX)/man/man1/xautoclick.1

