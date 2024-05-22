# MPIvdif2psrfits

https://github.com/xuanyuanstar/MPIvdif2psrfits

Software routines to convert VLBI VDIF format into pulsar search mode data in PSRFITS format, using Open MPI functionality. 

# Installation
 * Install `cfitsio` and `fftw3f` libraries, as well as OpenMPI. The code has been used with cfitsio 3.45 and fftw 3.3.4.
 * If a different `template` directory (with PSRFITS header templates) should be used, other than the included directory, then update Makefile.am `PSRFITS_TEMPLATE_DIR`
 * To build:

        module load openmpi/4.0.4
        autoreconf --install --force
        automake --add-missing --copy
        CC=mpicc CXX=mpicxx ./configure --prefix=/home/oper/.local/ --with-cfitsio=/cluster/cfitsio/cfitsio-3.45-bin/

 * Run "make && make install".

# MPIvditf2psrfitsALMA
Dealing with ALMA VDIF output, 32 x 62.5 MHz channels

Usage:
 * -f   Observing central frequency (MHz)
 * -i   Input vdif pol0
 * -j   Input vdif pol1
 * -T   Telescope name (by default ALMA)
 * -b   Band sense (-1 for lower-side, 1 for upper-side, by default 1)
 * -s   Seconds to get statistics to fill in invalid frames
 * -t   Time sample scrunch factor (by default 1). One time sample 8 microsecond
 * -S   Name of the source (by default J0835-4510)
 * -r   RA of the source (AA:BB:CC.DD)
 * -c   Dec of the source (+AA:BB:CC.DD)
 * -D   Ouput data status (I for Stokes I, C for coherence product, X for pol0 I, Y for pol1 I, S for Stokes, P for polarised signal, S for stokes, by default C)
 * -k   Buffer size of each dataread (in MB, by default 1000 MB)
 * -v   Verbose
 * -O   Route of the output file 
 * -h   Available options

Example of a command:

        mpirun --hostfile host_file MPIvdif2psrfitsALMA -f 227068.75 \
        -i e18c21_Aa_No0005.vdif -j e18c21_Aa_No0005.vdif -T ALMA \
        -b 1 -s 0.1 -S SgrA -r 17:45:40 -c -29:00:28 -t 1 -O ~/output/ -D S

# MPIvdif2psrfitsPico
Dealing with Pico & LMT VDIF output, 1 x 2 GHz channel

Usage:
 * -f   Observing central frequency (MHz)
 * -i   Input vdif pol0
 * -j   Input vdif pol1
 * -T   Telescope name (by default Pico Veleta)
 * -b   Band sense (-1 for lower-side, 1 for upper-side, by default 1)
 * -s   Seconds to get statistics to fill in invalid frames
 * -t   Time sample scrunch factor (by default 1). One time sample 8 microsecond
 * -S   Name of the source (by default J0835-4510)
 * -r   RA of the source (AA:BB:CC.DD)
 * -c   Dec of the source (+AA:BB:CC.DD)
 * -D   Ouput data status (I for Stokes I, C for coherence product, X for pol0 I, Y for pol1 I, S for Stokes, P for polarised signal, S for stokes, by default C)
 * -n   Number of channels kept (Power of 2 up to 4096, by default 1)
 * -k   Buffer size of each dataread (in MB, by default 1000 MB)
 * -v   Verbose
 * -O   Route of the output file 
 * -h   Available options

Example of a command:

        mpirun --hostfile host_file MPIvdif2psrfitsPico -f 227068.75 \
        -i e17c07_Lm_097-0625.vdif -j e17c07_Lm_097-0625.vdif -T LMT \
        -b 1 -s 0.1 -S SgrA -r 17:45:40 -c -29:00:28 -t 1 -n 16 -O ~/output/ -D S

Example Environment

        module load slurm
        salloc -p compute -n 32 --nodelist=mark6-11,node02,node03,node04,node05
        
        # SLURM<->OpenMPI allocation exchange a bit messy, easier to get rid of SLURM infos,
        # and let OpenMPI do the process to host mapping:
        unset SLURM_JOBID
        
        module load openmpi/4.0.4/gcc/4.8.2
        
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/cluster/cfitsio/cfitsio-3.45-bin/lib/
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/oper/.local/lib/
        
        export PATH=$PATH:/home/oper/.local/bin/
        
        export HWLOC_HIDE_ERRORS=1
        export OMPI_MCA_btl_openib_if_include="mlx4_0:1"
        export OMPI_MCA_btl_openib_allow_ib=1
        export OMPI_MCA_regx=naive
        export MPI_OPTS="--map-by slot --bind-to none"
        export MPI_OPTS="$MPI_OPTS -x LD_LIBRARY_PATH"
        export MPI_OPTS="$MPI_OPTS --mca btl_tcp_if_include 10.10.0.0/16 --mca oob_tcp_if_include 10.10.0.0/16"
        export MPI_OPTS="$MPI_OPTS --mca btl_openib_ib_timeout 20 -mca btl_openib_ib_min_rnr_timer 25"
        export MPI_OPTS="$MPI_OPTS --display-allocation -mca rmaps_base_verbose 5  --report-bindings --show-progress"
        
        # Produce a MPI machines file
        # - the Mark6 playback unit occurs only once
        # - each compute node occurs 20 time i.e. the number of CPU cores on each node
        machinesfile=`mktemp /tmp/psrjob_mpi_machines.XXXXX`
        echo "mark6-11 slots=1" > $machinesfile
        numproc=1
        for n in node02 node03 node04 node05; do
                echo "$n slots=20" >> $machinesfile
                numproc=$((numproc + 20))
        done
        
        # Test OpenMPI
        mpirun -np $numproc -hostfile $machinesfile $MPI_OPTS /usr/bin/hostname
        
        # Actual processing
        scanname=e22a26_Aa_085-0403.vdif
        psrfitsdir=/beegfs/pulsar/trackname/
        mkdir -p ${psrfitsdir}
        mpirun -np $numproc -hostfile $machinesfile $MPI_OPTS `which MPIvdif2psrfitsALMA` \
             -f 213068.75 -i /mark6-11_fuse/12/${scanname}.vdif -j /mark6-11_fuse/34/${scanname}.vdif \
             -T ALMA -b -1 -s 2.0 -S SgrA -r 17:45:40 -c -29:00:28 -t 1 -O ${psrfitsdir} -D S
