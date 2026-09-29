# $Id: Makefile.in,v 1.51 2009/03/27 07:24:26 mk Exp $

ACLOCAL_AMFLAGS = -I m4

AM_CPPFLAGS =	-I. -I./extra
AM_CPPFLAGS +=	@CPPFLAGS@
AM_CPPFLAGS += -DSYSCONFDIR=\"${sysconfdir}\"
AM_CPPFLAGS += -DDEBUG
AM_CPPFLAGS += -DDEBUG_TRACE
# AM_CPPFLAGS += -DSELFTEST
AM_CPPFLAGS +=  @DEFS@
# AM_CPPFLAGS += -DSADB_X_MIGRATE

TESTS = eaytest test_purge_remote test_natoa test_ts_substitute test_ip_any
check_PROGRAMS = eaytest test_purge_remote test_natoa test_ts_substitute \
	test_ip_any
eaytest_SOURCES = eaytest.c
# eaytest uses the eay_* wrappers (crypto_openssl.c) and str2val(),
# which are iked objects, not part of libracoon.
eaytest_LDADD = crypto_openssl.$(OBJEXT) str2val.$(OBJEXT) $(LDADD)

# Unit tests link against all iked objects except main.o (the test
# provides its own main() and the globals normally defined there).
# -Iikev1 is needed to resolve the ikev1/* headers included by the test.
test_purge_remote_SOURCES = test_purge_remote.c
test_purge_remote_CPPFLAGS = $(AM_CPPFLAGS) -Iikev1
test_purge_remote_LDADD = $(filter-out main.$(OBJEXT),$(am_iked_OBJECTS)) $(LDADD)

# TODO item 5: NAT-OA payloads and the ID payload address helpers of the
# IKEv1 NAT-T transport mode code (ikev1/ikev1_natt.c).
test_natoa_SOURCES = test_natoa.c
test_natoa_CPPFLAGS = $(AM_CPPFLAGS) -Iikev1
test_natoa_LDADD = $(filter-out main.$(OBJEXT),$(am_iked_OBJECTS)) $(LDADD)

# TODO item 6: address substitution on the traffic selectors of IKEv2
# (RFC 7296 2.23.1, ike_conf.c).
test_ts_substitute_SOURCES = test_ts_substitute.c
test_ts_substitute_LDADD = $(filter-out main.$(OBJEXT),$(am_iked_OBJECTS)) $(LDADD)

# TODO item 8: the IP_ANY (formerly IP_RW) wildcard of ike_conf.c,
# i.e. ike_compare_id(), ike_determine_sa_endpoint() and selector
# lookup with an IP_ANY address.
test_ip_any_SOURCES = test_ip_any.c
test_ip_any_LDADD = $(filter-out main.$(OBJEXT),$(am_iked_OBJECTS)) $(LDADD)

IKEV1_SRC= ikev1/ikev1.c ikev1/genlist.c ikev1/ipsec_doi.c ikev1/isakmp_ident.c ikev1/isakmp_quick.c ikev1/isakmp_inf.c ikev1/handler.c ikev1/strnames.c ikev1/oakley.c ikev1/vendorid.c ikev1/algorithm.c ikev1/proposal.c  ikev1/pfkey.c
IKEV1_NATT_SRC = ikev1/ikev1_natt.c

IKEV2_SRC = ikev2.c ikev2_auth.c ikev2_child.c ikev2_cookie.c ikev2_notify.c ikev2_packet.c ikev2_payload.c ikev2_proposal.c ikev2_rekey.c ikev2_config.c ikev2_frag.c
IKEV2_NATT_SRC = nattraversal.c

iked_SOURCES = main.c dh.c ike_sa.c ike_conf.c isakmp.c authenticator.c encryptor.c crypto_openssl.c str2val.c keyed_hash.c sockmisc.c ike_pfkey.c ike_spmif.c schedule.c ratelimit.c script.c rtsock.c ${IKEV1_SRC} ${IKEV2_SRC} ${IKEV1_NATT_SRC} ${IKEV2_NATT_SRC}
sbin_PROGRAMS=iked

LIBS = @LIBS@

man_MANS = iked.8
