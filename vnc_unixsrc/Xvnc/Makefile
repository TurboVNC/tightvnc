# $XConsortium: Makefile /main/36 1996/11/13 14:42:50 lehors $
# $XFree86: xc/Makefile,v 3.6.2.2 1998/02/07 00:44:32 dawes Exp $

# Luna users will need to either run make as "make MAKE=make"
# or add "MAKE = make" to this file.

RELEASE = "Release 6.3"
SHELL = /bin/sh
RM = rm -f
MV = mv
WORLDOPTS = -k
WIN32WORLDOPTS = -i
TOP = .
CURRENT_DIR = .
CONFIGSRC = $(TOP)/config
IMAKESRC = $(CONFIGSRC)/imake
DEPENDSRC = $(CONFIGSRC)/makedepend
DEPENDTOP = ../..
IMAKETOP = ../..
IRULESRC = $(CONFIGSRC)/cf
IMAKE = $(IMAKESRC)/imake
IMAKE_CMD = $(IMAKE) -I$(IRULESRC) $(IMAKE_DEFINES)
MAKE_OPTS = -f xmakefile
MAKE_CMD = $(MAKE) $(MAKE_OPTS)
FLAGS = $(MFLAGS) -f Makefile.ini BOOTSTRAPCFLAGS="$(BOOTSTRAPCFLAGS)"

all:
	@$(MAKE_CMD) xmakefile-exists || $(MAKE) all-initial
	@$(MAKE_CMD) $@

all-initial:
	@echo Please use make World, or on NT use nmake World.Win32.
	@echo Check the configuration parameters in the $(IRULESRC) directory,
	@echo and pass BOOTSTRAPCFLAGS if necessary.
	@echo Read the release notes carefully before proceeding.
	@echo Do not name your log file make.log or it will be deleted.

World:
	@echo ""
	@echo Building $(RELEASE) of the X Window System.
	@echo ""
	@case "x$(BOOTSTRAPCFLAGS)" in x) \
	echo I hope you checked the configuration parameters in $(IRULESRC) ; \
	echo to see if you need to pass BOOTSTRAPCFLAGS. ; \
	echo "" ; \
	;; esac;
	@date
	@echo ""
	@if [ ! -f $(IRULESRC)/host.def ]; then \
	    if [ ! -f $(TOP)/lib/Xt/Imakefile ]; then \
		echo "#define BuildServersOnly YES" > $(IRULESRC)/host.def; \
	    else \
		if [ ! -f $(TOP)/fonts/Imakefile ]; then \
		    echo "#define BuildFonts NO" > $(IRULESRC)/host.def; \
		else \
		    echo "" > $(IRULESRC)/host.def; \
		fi \
	    fi \
	fi
	cd $(IMAKESRC) && $(MAKE) $(FLAGS) clean
	$(MAKE) $(MFLAGS) Makefile.boot
	$(MAKE_CMD) $(MFLAGS) VerifyOS
	$(MAKE_CMD) $(MFLAGS) Makefiles
	$(MAKE_CMD) $(MFLAGS) clean BOOTSTRAPSUBDIRS=
	$(MAKE_CMD) $(MFLAGS) includes
	$(MAKE_CMD) $(MFLAGS) depend
	$(MAKE_CMD) $(MFLAGS) $(WORLDOPTS) World
	@echo ""
	@date
	@echo ""
	@echo Full build of $(RELEASE) of the X Window System complete.
	@echo ""

.PRECIOUS: Makefile

# This is just a sequence of bootstrapping steps we have to do.
# The steps are listed as separate targets so clearmake can wink in
# the Makefile.proto files.
Makefile.boot: imake.proto $(DEPENDSRC)/Makefile.proto depend.bootstrap $(IMAKESRC)/Makefile.proto imake.bootstrap

imake.proto:
	cd $(IMAKESRC) && $(MAKE) $(FLAGS)
	$(RM) $(DEPENDSRC)/Makefile.proto

$(DEPENDSRC)/Makefile.proto:
	$(IMAKE_CMD) -s $(DEPENDSRC)/Makefile.proto -f $(DEPENDSRC)/Imakefile -DTOPDIR=$(DEPENDTOP) -DCURDIR=$(DEPENDSRC)

depend.bootstrap:
	cd $(DEPENDSRC) && $(RM) -r Makefile Makefile.dep makedepend *.o bootstrap
	cd $(DEPENDSRC) && $(MAKE) -f Makefile.proto bootstrap

$(IMAKESRC)/Makefile.proto:
	$(IMAKE_CMD) -s $(IMAKESRC)/Makefile.proto -f $(IMAKESRC)/Imakefile -DTOPDIR=$(IMAKETOP) -DCURDIR=$(IMAKESRC)

imake.bootstrap:
	cd $(IMAKESRC) && $(MAKE) -f Makefile.proto bootstrapdepend
	cd $(IMAKESRC) && $(MAKE) $(FLAGS) bootstrap
	cd $(IMAKESRC) && $(MAKE) -f Makefile.proto all 
	-@if [ -f xmakefile ]; then set -x; \
	  $(RM) xmakefile.bak; $(MV) xmakefile xmakefile.bak; \
	  else exit 0; fi
	$(MAKE) $(MFLAGS) xmakefile

Makefile::
	$(MAKE) $(MFLAGS) xmakefile

xmakefile: Imakefile
	$(IMAKE_CMD) -s xmakefile -DTOPDIR=$(TOP) -DCURDIR=$(CURRENT_DIR)

World.Win32:
	@echo :
	@echo Building $(RELEASE) of the X Window System.
	@echo :
	@echo :
	-@if not exist $(IRULESRC)\host.def echo > $(IRULESRC)\host.def
	cd $(IMAKESRC)
	$(MAKE) -f Makefile.ini clean.Win32
	$(MAKE) -f Makefile.ini imake.exe
	cd ..\..
	-if exist xmakefile.bak del xmakefile.bak
	-if exist xmakefile ren xmakefile xmakefile.bak
	$(IMAKE:/=\) -s xmakefile -I$(IRULESRC) $(IMAKE_DEFINES) -DTOPDIR=$(TOP) -DCURDIR=$(CURRENT_DIR)
	$(MAKE_CMD) $(MFLAGS) VerifyOS
	$(MAKE_CMD) $(MFLAGS) Makefiles
	$(MAKE_CMD) $(MFLAGS) clean
	cd $(CONFIGSRC)\util
	$(MAKE) mkdirhier.exe
	cd ..\..
	$(MAKE_CMD) $(MFLAGS) includes
	$(MAKE_CMD) $(MFLAGS) depend
	$(MAKE_CMD) $(MFLAGS) $(WIN32WORLDOPTS)
	@echo :
	@echo :
	@echo Full build of $(RELEASE) of the X Window System complete.
	@echo :

World.OS2:
	@echo :
	@echo Building $(RELEASE) of the X Window System on OS/2.
	@echo :
	@echo :
	-@if not exist $(IRULESRC)\host.def echo > $(IRULESRC)\host.def
	\indir $(IMAKESRC) $(MAKE) SHELL= -f Makefile.ini clean.os2
	\indir $(IMAKESRC) $(MAKE) SHELL= CC=gcc -f Makefile.ini imake.os2
	-if exist Makefile.bak del Makefile.bak
	-if exist Makefile ren Makefile Makefile.bak
	$(subst /,\,$(IMAKE)) -I$(IRULESRC) $(IMAKE_DEFINES) -DTOPDIR=$(TOP) -DCURDIR=$(CURRENT_DIR)
	$(MAKE) $(MFLAGS) VerifyOS
	$(MAKE) $(MFLAGS) Makefiles
	$(MAKE) $(MFLAGS) clean
	$(MAKE) $(MFLAGS) includes
	$(MAKE) $(MFLAGS) depend
	$(MAKE) $(MFLAGS)  
	@echo :
	@echo :
	@echo Full build of $(RELEASE) of the X Window System complete.
	@echo :

# don't allow any default rules in this Makefile
.SUFFIXES:
# quiet "make" programs that display a message if suffix list empty
.SUFFIXES: .Dummy

# a copy of every rule that might be invoked at top level

clean:
	    $(MAKE_CMD) $@
dangerous_strip_clean:
	    $(MAKE_CMD) $@
depend:
	    $(MAKE_CMD) $@
Everything:
	    $(MAKE_CMD) $@
external.ln:
	    $(MAKE_CMD) $@
includes:
	    $(MAKE_CMD) $@
install.linkkit:
	    $(MAKE_CMD) $@
install.ln:
	    $(MAKE_CMD) $@
install.man:
	    $(MAKE_CMD) $@
install:
	    $(MAKE_CMD) $@
Makefiles:
	    $(MAKE_CMD) $@
man_keywords:
	    $(MAKE_CMD) $@
tags:
	    $(MAKE_CMD) $@
VerifyOS:
	    $(MAKE_CMD) $@
