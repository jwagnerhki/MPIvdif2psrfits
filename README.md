# MPIvdif2psrfits

https://github.com/xuanyuanstar/MPIvdif2psrfits

Software routines to convert VLBI VDIF format into pulsar search mode data in PSRFITS format, using Open MPI functionality. 

# Installation
 * Install cfitsio and fftw library. The code has been used with cfitsio3410 and fftw3.3.4.;
 * Modify "Makefile.am" to specify route to the source code (where it finds the PSRFITs header template);
 * In the "MPIvdif2psrfits/" directory, run "./bootstrap";
 * Run "./configure CC=mpicc". You may need to specify links to cfitsio and fftw library manually, e.g.:

./configure --prefix=/home/kliu/Soft/ --with-cfitsio-lib-dir=/home/kliu/Soft/lib/ --with-cfitsio-include-dir=/home/kliu/Soft/include/ CC=mpicc

 * Run "make && make install".

# MPIvditf2psrfitsALMA
# Dealing with ALMA VDIF output, 32 x 62.5 MHz channels
# Usage
  -f   Observing central frequency (MHz)
  -i   Input vdif pol0
  -j   Input vdif pol1
  -T   Telescope name (by default ALMA)
  -b   Band sense (-1 for lower-side, 1 for upper-side, by default 1)
  -s   Seconds to get statistics to fill in invalid frames
  -t   Time sample scrunch factor (by default 1). One time sample 8 microsecond
  -S   Name of the source (by default J0835-4510)
  -r   RA of the source (AA:BB:CC.DD)
  -c   Dec of the source (+AA:BB:CC.DD)
  -D   Ouput data status (I for Stokes I, C for coherence product, X for pol0 I, Y for pol1 I, S for Stokes, P for polarised signal, S for stokes, by default C)
  -k   Buffer size of each dataread (in MB, by default 1000 MB)
  -v   Verbose
  -O   Route of the output file 
  -h   Available options

Example of a command:
 * mpirun --hostfile host_file MPIvdif2psrfitsALMA -f 227068.75 -i e18c21_Aa_No0005.vdif -j e18c21_Aa_No0005.vdif -T ALMA -b 1 -s 0.1 -S SgrA -r 17:45:40 -c -29:00:28 -t 1 -O ~/output/ -D S

# MPIvdif2psrfitsPico
# Dealing with Pico & LMT VDIF output, 1 x 2 GHz channel
# Usage
  -f   Observing central frequency (MHz)
  -i   Input vdif pol0
  -j   Input vdif pol1
  -T   Telescope name (by default Pico Veleta)
  -b   Band sense (-1 for lower-side, 1 for upper-side, by default 1)
  -s   Seconds to get statistics to fill in invalid frames
  -t   Time sample scrunch factor (by default 1). One time sample 8 microsecond
  -S   Name of the source (by default J0835-4510)
  -r   RA of the source (AA:BB:CC.DD)
  -c   Dec of the source (+AA:BB:CC.DD)
  -D   Ouput data status (I for Stokes I, C for coherence product, X for pol0 I, Y for pol1 I, S for Stokes, P for polarised signal, S for stokes, by default C)
  -n   Number of channels kept (Power of 2 up to 4096, by default 1)
  -k   Buffer size of each dataread (in MB, by default 1000 MB)
  -v   Verbose
  -O   Route of the output file 
  -h   Available options

Example of a command:
 * mpirun --hostfile host_file MPIvdif2psrfitsPico -f 227068.75 -i e17c07_Lm_097-0625.vdif -j e17c07_Lm_097-0625.vdif -T LMT -b 1 -s 0.1 -S SgrA -r 17:45:40 -c -29:00:28 -t 1 -n 16 -O ~/output/ -D S
