bin_PROGRAMS=PolyExpand
PolyExpand_SOURCES= \
	PolyExpand.cpp\
	PolyExpandMain.cpp

PolyExpand_CPPFLAGS=-I../include -I$(top_srcdir)/LIA_SpkTools/include -I$(ALIZE_PATH)/include -I$(top_srcdir)/include/Eigen
PolyExpand_LDFLAGS=-L$(top_srcdir)/lib -L$(ALIZE_PATH)/lib
PolyExpand_LDADD=-lliatools_${OS}_${ARCH}${DEBUG} $(lib_alize) $(LIBTHREAD)

all:
	mv ${bin_PROGRAMS} ${top_srcdir}/bin
