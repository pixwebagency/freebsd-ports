# $FreeBSD$

PORTNAME=	kholidays
DISTVERSION=	${KDE_FRAMEWORKS_VERSION}
CATEGORIES=	net kde kde-frameworks

MAINTAINER=	kde@FreeBSD.org
COMMENT=	KDE library for calendar holidays

LICENSE=	LGPL21

USES=		cmake:outsource compiler:c++11-lib gettext kde:5 tar:xz
USE_KDE=	ecm emoticons kdelibs4support init itemmodels itemviews
USE_QT5=	buildtools_build core gui qmake_build
USE_LDCONFIG=	yes

.include <bsd.port.mk>
