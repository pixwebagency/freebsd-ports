# $FreeBSD$

PORTNAME=	poudriere
DISTVERSION=	3.2.99.20180313
PORTREVISION=	0
CATEGORIES=	ports-mgmt
MASTER_SITES=	LOCAL/bdrewery/${PORTNAME}/ \
		http://mirror.shatow.net/freebsd/${PORTNAME}/ \
		GH
PKGNAMESUFFIX=	-devel

MAINTAINER=	bdrewery@FreeBSD.org
COMMENT=	Port build and test system

LICENSE=	BSD2CLAUSE

CONFLICTS_INSTALL=	poudriere-[0-9]*

USE_GITHUB=	yes
GH_ACCOUNT=	freebsd
GH_TAGNAME=	3.2.5-303-g3a844af55
#DISTNAME=	${GH_ACCOUNT}-${GH_PROJECT}-${GH_TAGNAME}

GNU_CONFIGURE=	yes
ETCDIR=		${PREFIX}/etc/poudriere.d

OPTIONS_DEFINE=	CERTS DIALOG4PORTS EXAMPLES QEMU ZSH
OPTIONS_DEFAULT=CERTS DIALOG4PORTS ZSH
OPTIONS_SUB=	yes

CERTS_DESC=		Install checksum and SSL certificates for jail creation
CERTS_RUN_DEPENDS+=	ca_root_nss>=0:security/ca_root_nss
CERTS_RUN_DEPENDS+=	freebsd-release-manifests>0:misc/freebsd-release-manifests

DIALOG4PORTS_DESC=	Install dialog4ports for options command
DIALOG4PORTS_RUN_DEPENDS=	dialog4ports>0:ports-mgmt/dialog4ports

QEMU_DESC=	Add qemu-user-static for non-x86 architectures
QEMU_RUN_DEPENDS=	qemu-user-static>0:emulators/qemu-user-static

.include <bsd.port.options.mk>

# rm -x support came in 10.0.  A bundled rm is used if not supported in base.
# While <10 is technically not supported in the Ports Tree, Poudriere
# detects this and it being a package build tool it should still be
# supported as a user may be installing this to build packages for upgrading
# their system to a supported release.
.if ${OSVERSION} < 1000000
PLIST_SUB+=	RM=""
.else
PLIST_SUB+=	RM="@comment "
.endif

post-patch:
	${CP} -f ${WRKSRC}/src/bin/poudriere ${WRKSRC}/poudriere
	${REINPLACE_CMD} \
	    -e "s,^\(POUDRIERE_VERSION\)=.*,\1='${PKGVERSION}'," \
	    ${WRKSRC}/poudriere
	${REINPLACE_CMD} '/.PHONY: poudriere/d' ${WRKSRC}/Makefile.*

post-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}/share/zsh/site-functions/
	${INSTALL_DATA} ${WRKSRC}/completions/zsh/_poudriere \
	    ${STAGEDIR}${PREFIX}/share/zsh/site-functions/
	${LN} -fs ${LOCALBASE}/share/freebsd/MANIFESTS \
	    ${STAGEDIR}${DATADIR}/MANIFESTS

.include <bsd.port.mk>
