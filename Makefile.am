AM_DEFAULT_SOURCE_EXT = .cpp

providersdir = ${libdir}/ipmid-providers
providers_LTLIBRARIES =

sbin_PROGRAMS = \
	ipmid

ipmid_SOURCES = \
	ipmid.cpp \
	settings.cpp \
	host-cmd-manager.cpp \
	utils.cpp \
	oemrouter.cpp
nodist_ipmid_SOURCES = ipmiwhitelist.cpp

libipmi20_BUILT_LIST = \
	sensor-gen.cpp \
	inventory-sensor-gen.cpp \
	fru-read-gen.cpp \
	channel-gen.cpp

BUILT_SOURCES = \
	ipmiwhitelist.cpp \
	$(libipmi20_BUILT_LIST)

CLEANFILES = $(BUILT_SOURCES)

COMMON_CXX = \
	-flto \
	-Wno-psabi \
	$(SYSTEMD_CFLAGS) \
	$(libmapper_CFLAGS) \
	$(SDBUSPLUS_CFLAGS) \
	$(PHOSPHOR_LOGGING_CFLAGS) \
	$(PHOSPHOR_DBUS_INTERFACES_CFLAGS) \
	-DBOOST_ERROR_CODE_HEADER_ONLY \
	-DBOOST_SYSTEM_NO_DEPRECATED \
	-DBOOST_ALL_NO_LIB

ipmid_CXXFLAGS = $(COMMON_CXX)
ipmid_LDFLAGS = \
	$(SYSTEMD_LIBS) \
	$(libmapper_LIBS) \
	$(LIBADD_DLOPEN) \
	$(PHOSPHOR_LOGGING_LIBS) \
	$(PHOSPHOR_DBUS_INTERFACES_LIBS) \
	$(CRYPTO_LIBS) \
	-lstdc++fs \
	-pthread \
	-export-dynamic

# TODO: Rather than use -export-dynamic, we should use -export-symbol to have a
#       selective list of symbols.

ipmiwhitelist.cpp: ${srcdir}/generate_whitelist.sh $(WHITELIST_CONF)
	$(SHELL) $^ > $@

sensor-gen.cpp:
	$(AM_V_GEN)@SENSORGEN@ -o $(top_builddir) generate-cpp

inventory-sensor-gen.cpp:
	$(AM_V_GEN)@INVSENSORGEN@ -o $(top_builddir) generate-cpp

fru-read-gen.cpp:
	$(AM_V_GEN)@FRUGEN@ -o $(top_builddir) generate-cpp

channel-gen.cpp:
	$(AM_V_GEN)@CHANNELGEN@ -o $(top_builddir) generate-cpp

providers_LTLIBRARIES += libipmi20.la
libipmi20_la_SOURCES = \
	net.cpp \
	app/channel.cpp \
	app/watchdog.cpp \
	app/watchdog_service.cpp \
	apphandler.cpp \
	sys_info_param.cpp \
	sensorhandler.cpp \
	storagehandler.cpp \
	chassishandler.cpp \
	dcmihandler.cpp \
	ipmisensor.cpp \
	storageaddsel.cpp \
	transporthandler.cpp \
	globalhandler.cpp \
	groupext.cpp \
	utils.cpp \
	selutility.cpp \
	ipmi_fru_info_area.cpp \
	read_fru_data.cpp \
	sensordatahandler.cpp \
	$(libipmi20_BUILT_LIST)

@CODE_COVERAGE_RULES@

check_PROGRAMS =
XFAIL_TESTS =

TESTS = $(check_PROGRAMS)

libipmi20_la_LDFLAGS = \
	$(SYSTEMD_LIBS) \
	$(libmapper_LIBS) \
	$(PHOSPHOR_LOGGING_LIBS) \
	$(PHOSPHOR_DBUS_INTERFACES_LIBS) \
	-lstdc++fs \
	-version-info 0:0:0 -shared
libipmi20_la_CXXFLAGS = $(COMMON_CXX)

providers_LTLIBRARIES += libusercmds.la
libusercmds_la_LIBADD = user_channel/libuserlayer.la
libusercmds_la_SOURCES = \
	user_channel/usercommands.cpp \
	user_channel/channelcommands.cpp
libusercmds_la_LDFLAGS = \
	$(PHOSPHOR_LOGGING_LIBS) \
	$(LIBS) \
	-version-info 0:0:0 -shared
libusercmds_la_CXXFLAGS = \
	$(PHOSPHOR_LOGGING_CFLAGS) \
	-flto

providers_LTLIBRARIES += libsysintfcmds.la
libsysintfcmds_la_SOURCES = \
	systemintfcmds.cpp \
	host-interface.cpp
libsysintfcmds_la_LDFLAGS = \
	$(SYSTEMD_LIBS) \
	$(libmapper_LIBS) \
	$(PHOSPHOR_DBUS_INTERFACES_LIBS) \
	$(PHOSPHOR_LOGGING_LIBS) \
	$(SDBUSPLUS_LIBS) \
	-version-info 0:0:0 -shared
libsysintfcmds_la_CXXFLAGS = $(COMMON_CXX)

nobase_include_HEADERS = \
	host-ipmid/iana.hpp \
	user_channel/user_layer.hpp \
	user_channel/channel_layer.hpp \
	host-ipmid/ipmid-api.h \
	host-ipmid/ipmid-host-cmd.hpp \
	host-ipmid/ipmid-host-cmd-utils.hpp \
	host-ipmid/oemopenbmc.hpp \
	host-ipmid/oemrouter.hpp

# Forcing the build of self and then subdir
SUBDIRS = user_channel . test softoff
