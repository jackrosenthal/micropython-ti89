include ../../py/mkenv.mk

# qstr definitions (must come before including py.mk)
QSTR_DEFS = qstrdefsport.h

# include py core make definitions
include $(TOP)/py/py.mk

CC = tigcc
LD = tigcc
CPP = m68k-coff-tigcc-gcc -E

INC += -I$(TIGCC)/include/c
INC += -I.
INC += -I$(TOP)
INC += -I$(BUILD)
INC += -Ifakeincludes

COPT = -O3
CFLAGS =  $(INC) -Wall -std=gnu99 $(COPT) -DUSE_TI89 -DOPTIMIZE_ROM_CALLS -DMIN_AMS=204
LDFLAGS = -Wl,--optimize-code

LIBS =

SRC_C = \
	main.c \
	lib/utils/stdout_helpers.c \
	lib/utils/pyexec.c \
	lib/mp-readline/readline.c \

OBJ = $(PY_CORE_O) $(addprefix $(BUILD)/, $(SRC_C:.c=.o))

all: $(BUILD)/micropython.89z

$(BUILD)/micropython.89z: $(OBJ)
	$(ECHO) "LINK $@"
	$(Q)$(LD) $(LDFLAGS) -o $@ $^ $(LIBS)

include $(TOP)/py/mkrules.mk

# fix this function to put the dependency file in correct location for
# ancient GCC
define compile_c
$(ECHO) "CC $<"
$(Q)$(CC) $(CFLAGS) -c -MD -MF$(@:.o=.d) -o $@ $<
@# The following fixes the dependency file.
@# See http://make.paulandlesley.org/autodep.html for details.
@# Regex adjusted from the above to play better with Windows paths, etc.
@$(CP) $(@:.o=.d) $(@:.o=.P); \
  $(SED) -e 's/#.*//' -e 's/^.*:  *//' -e 's/ *\\$$//' \
      -e '/^$$/ d' -e 's/$$/ :/' < $(@:.o=.d) >> $(@:.o=.P); \
  $(RM) -f $(@:.o=.d)
endef
