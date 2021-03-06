noinst_LTLIBRARIES = \
	libdict.la \
	libdict_lua.la

AM_CPPFLAGS = \
	-I$(top_srcdir)/src/lib \
	-I$(top_srcdir)/src/lib-test \
	-I$(top_srcdir)/src/lib-settings

base_sources = \
	dict.c \
	dict-client.c \
	dict-file.c \
	dict-memcached.c \
	dict-memcached-ascii.c \
	dict-redis.c \
	dict-fail.c \
	dict-transaction-memory.c

libdict_la_SOURCES = \
	$(base_sources)

headers = \
	dict.h \
	dict-client.h \
	dict-private.h \
	dict-transaction-memory.h

if HAVE_LUA
libdict_lua_la_SOURCES =

# Internally, the dict methods yield via lua_yieldk() as implemented in Lua
# 5.3 and newer.
if DLUA_WITH_YIELDS
libdict_lua_la_SOURCES += \
	dict-lua.c \
	dict-iter-lua.c \
	dict-txn-lua.c
libdict_lua_la_CPPFLAGS = \
	$(AM_CPPFLAGS) \
	$(LUA_CFLAGS) \
	-I$(top_srcdir)/src/lib-lua
libdict_lua_la_LIBADD =
libdict_lua_la_DEPENDENCIES = \
	libdict.la
endif

headers += \
	dict-lua.h \
	dict-lua-private.h
endif

pkginc_libdir=$(pkgincludedir)
pkginc_lib_HEADERS = $(headers)

test_programs = \
	test-dict

noinst_PROGRAMS = $(test_programs) test-dict-client

test_libs = \
	../lib-test/libtest.la \
	../lib/liblib.la

test_dict_SOURCES = test-dict.c
test_dict_LDADD = libdict.la $(test_libs)
test_dict_DEPENDENCIES = $(noinst_LTLIBRARIES) $(test_libs)

test_dict_client_SOURCES = test-dict-client.c
test_dict_client_LDADD = $(noinst_LTLIBRARIES) ../lib/liblib.la
test_dict_client_DEPENDENCIES = $(noinst_LTLIBRARIES) $(test_libs)

check-local:
	for bin in $(test_programs) $(check_PROGRAMS); do \
	  if ! $(RUN_TEST) ./$$bin; then exit 1; fi; \
	done
