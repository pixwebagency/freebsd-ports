# Created by: Vlad Galu <galu@packetdam.com>
# $FreeBSD$

PORTNAME=	arangodb
DISTVERSIONPREFIX=	v
DISTVERSION=	3.2.10
CATEGORIES=	databases net
PKGNAMESUFFIX=	32

MAINTAINER=	galu@packetdam.com
COMMENT=	ArangoDB is a distributed NoSQL database with multiple data models

LICENSE=	APACHE20
LICENSE_FILE=	${WRKSRC}/LICENSE

ONLY_FOR_ARCHS=	amd64
ONLY_FOR_ARCHS_REASON=	"Only builds on amd64"
BROKEN_FreeBSD_10=	libc++ is too old
USES=		gmake ssl cmake:outsource,noninja python:2.7 compiler
USE_GITHUB=	yes

CMAKE_ARGS=	-DUSE_JEMALLOC:BOOL=off \
		-DUSE_BACKTRACE:BOOL=on \
		-DBASE_LD_FLAGS:STRING="-L${BUILD_WRKSRC}/3rdParty/V8/v5.7.492.77/x64.release/obj.target/src -L${BUILD_WRKSRC}/3rdParty/V8/v5.7.492.77/x64.release/obj.target/third_party/icu -L${LOCALBASE}/lib -L/usr/lib"

USERS=		arangodb
GROUPS=		arangodb
USE_RC_SUBR=	arangod

post-install:
.for section in 1 8
	${MV} ${STAGEDIR}${PREFIX}/share/man/man${section}/* ${STAGEDIR}${PREFIX}/man/man${section}/
	${RMDIR} ${STAGEDIR}${PREFIX}/share/man/man${section}
.endfor
	${RMDIR} ${STAGEDIR}${PREFIX}/share/man
	${REINPLACE_CMD} -e 's|/usr/local/var/lib/arangodb3|/var/db/arangodb|g' ${STAGEDIR}${PREFIX}/etc/arangodb/*.conf
	${REINPLACE_CMD} -e 's|/usr/local/var/log/arangodb3|/var/log/arangodb|g' ${STAGEDIR}${PREFIX}/etc/arangodb/arangod.conf
	${FIND} ${STAGEDIR}${PREFIX}/etc/arangodb/ -type f -name *.conf -exec ${MV} {} {}.sample \;
	${RMDIR} ${STAGEDIR}${PREFIX}/var/lib/arangodb
	${RMDIR} ${STAGEDIR}${PREFIX}/var/lib/arangodb-apps
	${RMDIR} ${STAGEDIR}${PREFIX}/var/lib
	${RMDIR} ${STAGEDIR}${PREFIX}/var/log/arangodb
	${RMDIR} ${STAGEDIR}${PREFIX}/var/log
	${RMDIR} ${STAGEDIR}${PREFIX}/var

.include <bsd.port.mk>
