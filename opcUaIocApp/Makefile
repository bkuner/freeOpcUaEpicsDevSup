TOP=..
include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS AFTER THIS LINE

#------------------------generic definitions----------------------#

CROSS_COMPILER_TARGET_ARCHS=

STATIC_BUILD = YES

DB = freeopcuaTEST.db

IOCS+= OPCUAIOC

PROD_IOC = $(IOCS)
DBD = $(IOCS:%=%.dbd)

define RRDD
$(ioc)_SRCS += $(ioc)_registerRecordDeviceDriver.cpp
endef

$(foreach ioc,$(PROD_IOC),$(eval $(RRDD)))

PROD_SRCS += SoftIocMain.cpp

PROD_LIBS += $(EPICS_BASE_IOC_LIBS) opcUa

LIBRARY_IOC += opcUa
LIBOBJS += devOpcUa drvOpcUa
opcUa_OBJS = $(LIBOBJS)

###

USR_CFLAGS += -std=c1x -ggdb
USR_CXXFLAGS += -ggdb -std=c++11
USR_SYS_LIBS += dl pthread xml2 boost_system 
USR_SYS_LIBS += opcuaclient opcuaprotocol opcuacore
USR_LDFLAGS  += -L$(FREEOPCUA)/.libs

USR_INCLUDES += -I$(FREEOPCUA)/include
USR_INCLUDES += -I/usr/include/libxml2
USR_LDFLAGS += '-Wl,-rpath,$(FREEOPCUA)/.libs'
###

INSTALL_BOOT =$(TOP)/iocBoot
STCMD_INSTALLS = $(IOCS:%=$(INSTALL_BOOT)/ioc%/st.cmd)

# stuff that is used by all IOCs

Standard_DBD += base.dbd
#Standard_DBD += caPutLog.dbd
Standard_DBD += iocshGlobalCommands.dbd

#Standard_LIBS += caPutLog

# IOC specific definitions go here

#-----------------------------OPCUAIOC-----------------------------#

OPCUAIOC_DBD += $(Standard_DBD)
OPCUAIOC_DBD += opcUa.dbd

OPCUAIOC_LIBS += $(Standard_LIBS)opcUa

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

build: $(STCMD_INSTALLS)

vpath st.cmd.% $(GENERIC_SRC_DIRS) $(COMMON_DIR)

# install generated st.cmd files
$(STCMD_INSTALLS): $(INSTALL_BOOT)/ioc%/st.cmd: st.cmd.%
	$(ECHO) "Installing $@"
	@$(INSTALL) -d -m 755 $< $(@D)
	@$(MV) $(@D)/st.cmd.$* $@
