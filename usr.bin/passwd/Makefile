#	$NetBSD: Makefile,v 1.45 2017/01/28 23:44:51 christos Exp $
#	from: @(#)Makefile    8.3 (Berkeley) 4/2/94

.include <bsd.own.mk>

USE_FORT?= yes	# setuid
PROG=	passwd
SRCS=	local_passwd.c passwd.c
MAN=	passwd.1

CPPFLAGS+=-I${.CURDIR} -DLOGIN_CAP

.if (${USE_YP} != "no")
SRCS+=	yp_passwd.c
CPPFLAGS+=-DYP
DPADD+=	${LIBRPCSVC}
LDADD+=	-lrpcsvc
LINKS+=	${BINDIR}/passwd ${BINDIR}/yppasswd
MAN+=	yppasswd.1
.endif

DPADD+= ${LIBCRYPT} ${LIBUTIL}
LDADD+= -lcrypt -lutil

BINOWN=	root
BINMODE=4555

.if (${USE_KERBEROS} != "no")
CPPFLAGS+= -DKERBEROS5
SRCS+=	krb5_passwd.c

DPADD+=	${LIBKRB5} ${LIBCRYPTO} ${LIBASN1} ${LIBCOM_ERR} ${LIBROKEN} ${LIBCRYPT}
DPADD+=	${LIBSQLITE3}
LDADD+=	-lkrb5 -lcrypto -lasn1 -lcom_err -lroken -lcrypt
LDADD+=	-lsqlite3
.ifdef OVERRIDE_HEIMDAL_KPASSWD
LINKS+=	${BINDIR}/passwd ${BINDIR}/kpasswd
MAN+=	kpasswd.1
.endif
.endif

.if (${USE_PAM} != "no")
CPPFLAGS+=-DUSE_PAM
SRCS+= pam_passwd.c
LDADD+=-lpam ${PAM_STATIC_LDADD}
DPADD+=${LIBPAM} ${PAM_STATIC_DPADD}
.endif

.include <bsd.prog.mk>
