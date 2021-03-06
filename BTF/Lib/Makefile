#-------------------------------------------------------------------------------
# BTF Lib/Makefile
#-------------------------------------------------------------------------------

LIBRARY = libbtf
VERSION = 1.2.6
SO_VERSION = 1

default: library

include ../../SuiteSparse_config/SuiteSparse_config.mk

# BTF depends on SuiteSparse_config
LDLIBS += -lsuitesparseconfig

ccode: all

# compile and install in SuiteSparse/lib
library:
	$(MAKE) install INSTALL=$(SUITESPARSE)

# for testing only:
# TEST = -DTESTING

C = $(CC) $(CF)

INC = ../Include/btf.h ../Include/btf_internal.h

I = -I../Include -I../../SuiteSparse_config

all: library

OBJ = btf_order.o btf_maxtrans.o btf_strongcomp.o \
    btf_l_order.o btf_l_maxtrans.o btf_l_strongcomp.o

static: $(AR_TARGET)

$(AR_TARGET): $(OBJ)
	$(ARCHIVE) $@ $^
	- $(RANLIB) $@

$(OBJ): $(INC)

#-------------------------------------------------------------------------------

btf_order.o: ../Source/btf_order.c
	$(C) -c $(I) $< -o $@

btf_maxtrans.o: ../Source/btf_maxtrans.c
	$(C) -c $(I) $< -o $@

btf_strongcomp.o: ../Source/btf_strongcomp.c
	$(C) -c $(I) $< -o $@

#-------------------------------------------------------------------------------

btf_l_order.o: ../Source/btf_order.c
	$(C) -c $(I) -DDLONG $< -o $@

btf_l_maxtrans.o: ../Source/btf_maxtrans.c
	$(C) -c $(I) -DDLONG $< -o $@

btf_l_strongcomp.o: ../Source/btf_strongcomp.c
	$(C) -c $(I) -DDLONG $< -o $@

#-------------------------------------------------------------------------------

# install BTF
install: $(AR_TARGET) $(INSTALL_LIB)/$(SO_TARGET)

$(INSTALL_LIB)/$(SO_TARGET): $(OBJ)
	@mkdir -p $(INSTALL_LIB)
	@mkdir -p $(INSTALL_INCLUDE)
	@mkdir -p $(INSTALL_DOC)
	$(CC) $(SO_OPTS) $^ -o $@ $(LDLIBS)
	( cd $(INSTALL_LIB) ; ln -sf $(SO_TARGET) $(SO_PLAIN) )
	( cd $(INSTALL_LIB) ; ln -sf $(SO_TARGET) $(SO_MAIN) )
	$(CP) ../Include/btf.h $(INSTALL_INCLUDE)
	$(CP) ../README.txt $(INSTALL_DOC)/BTF_README.txt
	chmod 755 $(INSTALL_LIB)/$(SO_TARGET)
	chmod 644 $(INSTALL_INCLUDE)/btf.h
	chmod 644 $(INSTALL_DOC)/BTF_README.txt

uninstall:
	$(RM) $(INSTALL_LIB)/$(SO_TARGET)
	$(RM) $(INSTALL_LIB)/$(SO_PLAIN)
	$(RM) $(INSTALL_LIB)/$(SO_MAIN)
	$(RM) $(INSTALL_INCLUDE)/btf.h
	$(RM) $(INSTALL_DOC)/BTF_README.txt

#-------------------------------------------------------------------------------

purge: distclean

distclean: clean
	- $(RM) -r $(PURGE)

clean:
	- $(RM) -r $(CLEAN)
