#   Makefile: make program to compile the IDL interface
#
#   This file is part of NL-SAR Toolbox version 0.6.
#
#   Copyright Charles-Alban Deledalle (2013)
#   Email charles-alban.deledalle@math.u-bordeaux1.fr
#
#   This software is a computer program whose purpose is to provide a
#   suite of tools to manipulate SAR images.
#
#   This software is governed by the CeCILL license under French law and
#   abiding by the rules of distribution of free software. You can use,
#   modify and/ or redistribute the software under the terms of the CeCILL
#   license as circulated by CEA, CNRS and INRIA at the following URL
#   "http://www.cecill.info".
#
#   As a counterpart to the access to the source code and rights to copy,
#   modify and redistribute granted by the license, users are provided only
#   with a limited warranty and the software's author, the holder of the
#   economic rights, and the successive licensors have only limited
#   liability.
#
#   In this respect, the user's attention is drawn to the risks associated
#   with loading, using, modifying and/or developing or reproducing the
#   software by the user in light of its specific status of free software,
#   that may mean that it is complicated to manipulate, and that also
#   therefore means that it is reserved for developers and experienced
#   professionals having in-depth computer knowledge. Users are therefore
#   encouraged to load and test the software's suitability as regards their
#   requirements in conditions enabling the security of their systems and/or
#   data to be ensured and, more generally, to use and operate it in the
#   same conditions as regards security.
#
#   The fact that you are presently reading this means that you have had
#   knowledge of the CeCILL license and that you accept its terms.

MAKEFILE_RULES=../../Makefile.rules
MAKEFILE_DEPS=Makefile.deps

include $(MAKEFILE_RULES)

CFLAGS=$(CFLAGS_IDL) $(CFLAGS_COMMON)
LDFLAGS=-shared -Wall $(LDFLAGS_IDL) $(LDFLAGS_COMMON)
STATICLIBS=../../lib/libnlsartoolbox.a $(STATICLIBS_IDL) $(STATICLIBS_COMMON)

COMMON=	sarprintf.o sarwaitbar.o

MAIN=	libnlsartoolbox.o

DST=	$(COMMON) $(MAIN)
SRC=	$(DST:.o=.c)

TARGETS=$(MAIN:.o=.$(LDEXT))

ALL=$(TARGETS) *.dlm

.SUFFIXES : .c .o .so .$(LDEXT)

all:	$(TARGETS)

-include $(MAKEFILE_DEPS)

.c.$(LDEXT): $(<:.c=.o)

.c.o:
	$(CC) $(CFLAGS) -c $< -o $@

install:
	mkdir -p $(PREFIX_IDL)/lib/nlsar/
	mkdir -p $(IDL_DLM)/
	install -m 0644 *.pro $(PREFIX_IDL)/lib/nlsar/
	install -m 0644 $(ALL) $(IDL_DLM)/

depend:
	$(CC) -MM $(CFLAGS) $(SRC) > $(MAKEFILE_DEPS)
	for file in $(MAIN:.o=) ; do		\
		printf "$$file.$(LDEXT): $$file.o $(COMMON) ../../lib/libnlsartoolbox.a\n";		\
		printf "\t$(CC) -o $$file.$(LDEXT) $(COMMON) $$file.o $(STATICLIBS) $(LDFLAGS) \n\n";	\
	done >> $(MAKEFILE_DEPS)

clean:
	rm -f *.o *~ $(DST) $(TARGETS)

distclean: clean
	rm -f $(TARGETS) $(MAKEFILE_DEPS)
