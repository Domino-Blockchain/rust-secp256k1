.PHONY: clean-precomp precomp

ACLOCAL_AMFLAGS = -I build-aux/m4

# AM_CFLAGS will be automatically prepended to CFLAGS by Automake when compiling some foo
# which does not have an explicit foo_CFLAGS variable set.
AM_CFLAGS = $(SECP_CFLAGS)

lib_LTLIBRARIES = libsecp256k1.la
include_HEADERS = include/secp256k1.h
include_HEADERS += include/rustsecp256k1_v0_5_0_preallocated.h
noinst_HEADERS =
noinst_HEADERS += src/scalar.h
noinst_HEADERS += src/scalar_4x64.h
noinst_HEADERS += src/scalar_8x32.h
noinst_HEADERS += src/scalar_low.h
noinst_HEADERS += src/scalar_impl.h
noinst_HEADERS += src/scalar_4x64_impl.h
noinst_HEADERS += src/scalar_8x32_impl.h
noinst_HEADERS += src/scalar_low_impl.h
noinst_HEADERS += src/group.h
noinst_HEADERS += src/group_impl.h
noinst_HEADERS += src/ecdsa.h
noinst_HEADERS += src/ecdsa_impl.h
noinst_HEADERS += src/eckey.h
noinst_HEADERS += src/eckey_impl.h
noinst_HEADERS += src/ecmult.h
noinst_HEADERS += src/ecmult_impl.h
noinst_HEADERS += src/ecmult_compute_table.h
noinst_HEADERS += src/ecmult_compute_table_impl.h
noinst_HEADERS += src/ecmult_const.h
noinst_HEADERS += src/ecmult_const_impl.h
noinst_HEADERS += src/ecmult_gen.h
noinst_HEADERS += src/ecmult_gen_impl.h
noinst_HEADERS += src/ecmult_gen_compute_table.h
noinst_HEADERS += src/ecmult_gen_compute_table_impl.h
noinst_HEADERS += src/field_10x26.h
noinst_HEADERS += src/field_10x26_impl.h
noinst_HEADERS += src/field_5x52.h
noinst_HEADERS += src/field_5x52_impl.h
noinst_HEADERS += src/field_5x52_int128_impl.h
noinst_HEADERS += src/field_5x52_asm_impl.h
noinst_HEADERS += src/modinv32.h
noinst_HEADERS += src/modinv32_impl.h
noinst_HEADERS += src/modinv64.h
noinst_HEADERS += src/modinv64_impl.h
noinst_HEADERS += src/precomputed_ecmult.h
noinst_HEADERS += src/precomputed_ecmult_gen.h
noinst_HEADERS += src/assumptions.h
noinst_HEADERS += src/util.h
noinst_HEADERS += src/scratch.h
noinst_HEADERS += src/scratch_impl.h
noinst_HEADERS += src/selftest.h
noinst_HEADERS += src/testrand.h
noinst_HEADERS += src/testrand_impl.h
noinst_HEADERS += src/hash.h
noinst_HEADERS += src/hash_impl.h
noinst_HEADERS += src/field.h
noinst_HEADERS += src/field_impl.h
noinst_HEADERS += src/bench.h
noinst_HEADERS += src/basic-config.h
noinst_HEADERS += contrib/lax_der_parsing.h
noinst_HEADERS += contrib/lax_der_parsing.c
noinst_HEADERS += contrib/lax_der_privatekey_parsing.h
noinst_HEADERS += contrib/lax_der_privatekey_parsing.c

PRECOMPUTED_LIB = librustsecp256k1_v0_5_0_precomputed.la
noinst_LTLIBRARIES = $(PRECOMPUTED_LIB)
librustsecp256k1_v0_5_0_precomputed_la_SOURCES =  src/precomputed_ecmult.c src/precomputed_ecmult_gen.c
librustsecp256k1_v0_5_0_precomputed_la_CPPFLAGS = $(SECP_INCLUDES)

if USE_EXTERNAL_ASM
COMMON_LIB = librustsecp256k1_v0_5_0_common.la
else
COMMON_LIB =
endif
noinst_LTLIBRARIES += $(COMMON_LIB)

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libsecp256k1.pc

if USE_EXTERNAL_ASM
if USE_ASM_ARM
librustsecp256k1_v0_5_0_common_la_SOURCES = src/asm/field_10x26_arm.s
endif
endif

librustsecp256k1_v0_5_0_la_SOURCES = src/secp256k1.c
librustsecp256k1_v0_5_0_la_CPPFLAGS = -I$(top_srcdir)/include -I$(top_srcdir)/src $(SECP_INCLUDES)
librustsecp256k1_v0_5_0_la_LIBADD = $(SECP_LIBS) $(COMMON_LIB) $(PRECOMPUTED_LIB)
librustsecp256k1_v0_5_0_la_LDFLAGS = -no-undefined -version-info $(LIB_VERSION_CURRENT):$(LIB_VERSION_REVISION):$(LIB_VERSION_AGE)

if VALGRIND_ENABLED
librustsecp256k1_v0_5_0_la_CPPFLAGS += -DVALGRIND
endif

noinst_PROGRAMS =
if USE_BENCHMARK
noinst_PROGRAMS += bench bench_internal bench_ecmult
bench_SOURCES = src/bench.c
bench_LDADD = libsecp256k1.la $(SECP_LIBS) $(SECP_TEST_LIBS) $(COMMON_LIB)
bench_internal_SOURCES = src/bench_internal.c
bench_internal_LDADD = $(SECP_LIBS) $(COMMON_LIB) $(PRECOMPUTED_LIB)
bench_internal_CPPFLAGS = $(SECP_INCLUDES)
bench_ecmult_SOURCES = src/bench_ecmult.c
bench_ecmult_LDADD = $(SECP_LIBS) $(COMMON_LIB) $(PRECOMPUTED_LIB)
bench_ecmult_CPPFLAGS = $(SECP_INCLUDES)
endif

TESTS =
if USE_TESTS
noinst_PROGRAMS += tests
tests_SOURCES = src/tests.c
tests_CPPFLAGS = -I$(top_srcdir)/src -I$(top_srcdir)/include $(SECP_INCLUDES) $(SECP_TEST_INCLUDES)
if VALGRIND_ENABLED
tests_CPPFLAGS += -DVALGRIND
noinst_PROGRAMS += valgrind_ctime_test
valgrind_ctime_test_SOURCES = src/valgrind_ctime_test.c
valgrind_ctime_test_LDADD = libsecp256k1.la $(SECP_LIBS) $(COMMON_LIB)
endif
if !ENABLE_COVERAGE
tests_CPPFLAGS += -DVERIFY
endif
tests_LDADD = $(SECP_LIBS) $(SECP_TEST_LIBS) $(COMMON_LIB) $(PRECOMPUTED_LIB)
tests_LDFLAGS = -static
TESTS += tests
endif

if USE_EXHAUSTIVE_TESTS
noinst_PROGRAMS += exhaustive_tests
exhaustive_tests_SOURCES = src/tests_exhaustive.c
exhaustive_tests_CPPFLAGS = $(SECP_INCLUDES)
if !ENABLE_COVERAGE
exhaustive_tests_CPPFLAGS += -DVERIFY
endif
# Note: do not include $(PRECOMPUTED_LIB) in exhaustive_tests (it uses runtime-generated tables).
exhaustive_tests_LDADD = $(SECP_LIBS) $(COMMON_LIB)
exhaustive_tests_LDFLAGS = -static
TESTS += exhaustive_tests
endif

### Precomputed tables
EXTRA_PROGRAMS = precompute_ecmult precompute_ecmult_gen
CLEANFILES = $(EXTRA_PROGRAMS)

precompute_ecmult_SOURCES = src/precompute_ecmult.c
precompute_ecmult_CPPFLAGS = $(SECP_INCLUDES)
precompute_ecmult_LDADD = $(SECP_LIBS) $(COMMON_LIB)

precompute_ecmult_gen_SOURCES = src/precompute_ecmult_gen.c
precompute_ecmult_gen_CPPFLAGS = $(SECP_INCLUDES)
precompute_ecmult_gen_LDADD = $(SECP_LIBS) $(COMMON_LIB)

# See Automake manual, Section "Errors with distclean".
# We don't list any dependencies for the prebuilt files here because
# otherwise make's decision whether to rebuild them (even in the first
# build by a normal user) depends on mtimes, and thus is very fragile.
# This means that rebuilds of the prebuilt files always need to be
# forced by deleting them, e.g., by invoking `make clean-precomp`.
src/precomputed_ecmult.c:
	$(MAKE) $(AM_MAKEFLAGS) precompute_ecmult$(EXEEXT)
	./precompute_ecmult$(EXEEXT)
src/precomputed_ecmult_gen.c:
	$(MAKE) $(AM_MAKEFLAGS) precompute_ecmult_gen$(EXEEXT)
	./precompute_ecmult_gen$(EXEEXT)

PRECOMP = src/precomputed_ecmult_gen.c src/precomputed_ecmult.c
precomp: $(PRECOMP)

# Ensure the prebuilt files will be build first (only if they don't exist,
# e.g., after `make maintainer-clean`).
BUILT_SOURCES = $(PRECOMP)

maintainer-clean-local: clean-precomp

clean-precomp:
	rm -f $(PRECOMP)

EXTRA_DIST = autogen.sh SECURITY.md

if ENABLE_MODULE_ECDH
include src/modules/ecdh/Makefile.am.include
endif

if ENABLE_MODULE_RECOVERY
include src/modules/recovery/Makefile.am.include
endif

if ENABLE_MODULE_EXTRAKEYS
include src/modules/extrakeys/Makefile.am.include
endif

if ENABLE_MODULE_SCHNORRSIG
include src/modules/schnorrsig/Makefile.am.include
endif
