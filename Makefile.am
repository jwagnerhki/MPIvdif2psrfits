ACLOCAL_AMFLAGS = -I config
AM_CFLAGS = -I@top_srcdir@/ @CFITSIO_CFLAGS@ @FFTW3_CFLAGS@ @FFTW3F_CFLAGS@ -DPSRFITS_TEMPLATE_DIR=@abs_srcdir@/templates/
AM_LDADD = @CFITSIO_LIBS@ @FFTW3_LIBS@ @FFTW3F_LIBS@ 

bin_PROGRAMS = MPIvdif2psrfitsPico MPIvdif2psrfitsALMA calpsrfitsALMA
lib_LTLIBRARIES = libVDIF.la

libVDIF_la_SOURCES = dec2hms.c polyco.c vdifio.c read_psrfits.c write_psrfits.c cvrt2to8.c mjd2date.c getVDIFFrameDetection.c downsample.c nrutil.c gaussj.c
libVDIF_la_LIBADD = @CFITSIO_LIBS@ @FFTW3_LIBS@ @FFTW3F_LIBS@

MPIvdif2psrfitsPico_SOURCES = MPIvdif2psrfitsPico.c
MPIvdif2psrfitsPico_LDADD = libVDIF.la @CFITSIO_LIBS@ @FFTW3_LIBS@ @FFTW3F_LIBS@

MPIvdif2psrfitsALMA_SOURCES = MPIvdif2psrfitsALMA.c
MPIvdif2psrfitsALMA_LDADD = libVDIF.la @CFITSIO_LIBS@ @FFTW3_LIBS@ @FFTW3F_LIBS@

calpsrfitsALMA_SOURCES = calpsrfitsALMA.c libVDIF.la
calpsrfitsALMA_LDADD = libVDIF.la @CFITSIO_LIBS@ @FFTW3_LIBS@ @FFTW3F_LIBS@
