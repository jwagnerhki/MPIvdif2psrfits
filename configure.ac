AC_INIT([VDIF2psrfits], [1.0], [kliu@mpifr-bonn.mpg.de])
AC_CONFIG_MACRO_DIR([config])
AC_CONFIG_AUX_DIR([config])
AM_INIT_AUTOMAKE([-Wall foreign])

AM_PROG_AR
AC_PROG_CC([mpicc gcc cc])
#AC_PROG_CXX([mpicc])
#AC_PROG_F77
#AC_PROG_FC
#AC_PROG_F77_C_O
AX_MPI
AC_PROG_INSTALL
AC_PROG_LIBTOOL

AC_CHECK_LIB([m],[cos])

## Location of fftw3
AC_CHECK_LIB([fftw3], [fftwf_plan_dft_1d])
AC_CHECK_LIB([fftw3f], [fftwf_plan_dft_1d])
PKG_CHECK_MODULES([FFTW3], [fftw3], [], [AC_MSG_ERROR([fftw3 devel package not found])])
PKG_CHECK_MODULES([FFTW3F], [fftw3f], [], [AC_MSG_ERROR([fftw3f devel package not found])])

## Custom cfitsio via --with-cfitsion
AC_ARG_WITH([cfitsio],
    [AS_HELP_STRING([--with-cfitsio=<path>],
        [The top-level directory to the cfitsio installation.])],
    [AS_IF([test -d "$with_cfitsio"],
        [AC_MSG_NOTICE([Using cfitsio from "$with_cfitsio"])],
        [AC_MSG_ERROR([Specify an existing directory for cfitsio, not "$with_cfitsio"])])],
    [AC_MSG_ERROR([Use --with-cfitsio=<path> to specify the path to cfitsio])])
CFITSIO_ROOT="$with_cfitsio"
CFITSIO_LIBS="`pkg-config --libs $CFITSIO_ROOT/lib/pkgconfig/cfitsio.pc`"
#CFITSIO_LIBS="`pkg-config --libs-only-L $CFITSIO_ROOT/lib/pkgconfig/cfitsio.pc` libcfitsio.a"
CFITSIO_CFLAGS="`pkg-config --cflags $CFITSIO_ROOT/lib/pkgconfig/cfitsio.pc`"
AC_SUBST([CFITSIO_ROOT])
AC_SUBST(CFITSIO_LIBS)
AC_SUBST(CFITSIO_CFLAGS)

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
