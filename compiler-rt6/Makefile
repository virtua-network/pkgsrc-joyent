# $NetBSD$

.include "../../joyent/llvm6/version.mk"

DISTNAME=	compiler-rt-${LLVM_VERSION}.src
PKGNAME=	compiler-rt6-${LLVM_VERSION}
CATEGORIES=	lang devel
MASTER_SITES=	${LLVM_MASTER_SITE}
EXTRACT_SUFX=	.tar.xz

MAINTAINER=	pkgsrc-users@NetBSD.org
HOMEPAGE=	http://compiler-rt.llvm.org/
COMMENT=	LLVM runtime libraries
LICENSE=	modified-bsd OR mit

USE_LANGUAGES=		c99 c++11
USE_CMAKE=		yes
USE_SSP=		no
GCC_REQD+=		4.8

CONFIGURE_DIRS=		${WRKDIR}/build
CMAKE_ARG_PATH=		${WRKSRC}

OS_DIR.SunOS-5.11=	solaris2.11
OS_DIR=			${OS_DIR.${OPSYS}-${OS_VERSION}}

CMAKE_INSTALL_PREFIX=	${PREFIX}/llvm6

CMAKE_ARGS+=	-DCMAKE_BUILD_TYPE=Release
CMAKE_ARGS+=	-DCMAKE_C_COMPILER=${CC:Q}
CMAKE_ARGS+=	-DCMAKE_CXX_COMPILER=${CXX:Q}
CMAKE_ARGS+=	-DCOMPILER_RT_TEST_COMPILER=${PKG_CC:Q}
CMAKE_ARGS+=	-DCOMPILER_RT_OS_DIR:STRING=${OS_DIR}
CMAKE_ARGS+=	-DCOMPILER_RT_INSTALL_PATH:STRING=lib/clang/${LLVM_VERSION}
CMAKE_ARGS+=	-DCOMPILER_RT_INCLUDE_TESTS:BOOL=ON
CMAKE_ARGS+=	-DLLVM_CONFIG_PATH=${LLVM_CONFIG_PATH:Q}

PLIST_SUBST+=	COMPILER_RT_OS_DIR=${OS_DIR}

PLIST_VARS+=	asan ubsan stats fuzzer safestack cfi

TEST_TARGET=		check-compiler-rt

post-extract:
	${RUN} mkdir -p ${WRKDIR}/build

.include "../../mk/bsd.prefs.mk"

.if ${OPSYS} == "SunOS"
BUILD_DEPENDS+=		grep>=0:../../textproc/grep
TOOLS_PATH.grep=	${PREFIX}/bin/ggrep
.endif

.include "../../devel/googletest/buildlink3.mk"
.include "../../joyent/llvm6/buildlink3.mk"
.include "../../mk/bsd.pkg.mk"
