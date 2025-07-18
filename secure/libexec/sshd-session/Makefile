.include <src.opts.mk>
.include "${SRCTOP}/secure/ssh.mk"

PROG=	sshd-session
SRCS=	sshd-session.c auth-rhosts.c auth-passwd.c \
	audit.c audit-bsm.c audit-linux.c platform.c \
	sshpty.c sshlogin.c servconf.c serverloop.c \
	auth.c auth2.c auth2-methods.c auth-options.c session.c \
	auth2-chall.c groupaccess.c \
	auth-bsdauth.c auth2-hostbased.c auth2-kbdint.c \
	auth2-none.c auth2-passwd.c auth2-pubkey.c auth2-pubkeyfile.c \
	monitor.c monitor_wrap.c auth-krb5.c \
	auth2-gss.c gss-serv.c gss-serv-krb5.c \
	loginrec.c auth-pam.c auth-shadow.c auth-sia.c \
	sftp-server.c sftp-common.c \
	sandbox-null.c sandbox-rlimit.c sandbox-systrace.c sandbox-darwin.c \
	sandbox-seccomp-filter.c sandbox-capsicum.c sandbox-pledge.c \
	sandbox-solaris.c uidswap.c $(SKSRCS)
PACKAGE=	ssh

# gss-genr.c really belongs in libssh; see src/secure/lib/libssh/Makefile
SRCS+=	gss-genr.c

MAN=

# pam should always happen before ssh here for static linking
LIBADD=	pam ssh util

.if ${MK_AUDIT} != "no"
CFLAGS+= -DUSE_BSM_AUDIT=1 -DHAVE_GETAUDIT_ADDR=1
LIBADD+=	bsm
.endif

.if ${MK_BLACKLIST_SUPPORT} != "no"
CFLAGS+= -DUSE_BLACKLIST=1 -I${SRCTOP}/contrib/blocklist/include
SRCS+=	blacklist.c
LIBADD+= blacklist
LDFLAGS+=-L${LIBBLACKLISTDIR}
.endif

.if ${MK_GSSAPI} != "no" && ${MK_KERBEROS_SUPPORT} != "no"
LIBADD+=	gssapi_krb5 gssapi krb5
.if ${MK_MITKRB5} != "no"
.include "../../krb5/Makefile.inc"
CFLAGS+=	-I${KRB5_DIR}/include \
		-I${KRB5_SRCTOP}/include \
		-I${KRB5_OBJTOP}/lib \
		-I${KRB5_DIR}/lib/gssapi/generic \
		-I${KRB5_DIR}/lib/gssapi/krb5 \
		-I${KRB5_DIR}/lib/gssapi/mechglue
.endif
.endif

.if ${MK_TCP_WRAPPERS} != "no"
CFLAGS+=	-DLIBWRAP=1
LIBADD+=	wrap
.endif

LIBADD+=	crypto

.include <bsd.prog.mk>

.PATH:	${SSHDIR}
