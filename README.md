PerfTools
=========
*PerfTools* is a collection of performance tools for HPC environments.
The tools allow low-overhead measurement of CPU metrics, memory usage,
I/O and MPI. 

This suite consists of:

* [papiex](papiex/README.md) - provides a simple and uniform interface to collect 
detailed job, process and thread-level statistics using [PAPI](http://icl.cs.utk.edu//papi). 
* ioex - collects and displays I/O statistics in uninstrumented programs (coming soon!)
* [hpctoolkit](http://hpctoolkit.org/) - PerfTools provides a build of Rice University's
HPCToolKit, along with helper scripts to facilitate compatibility with PerfBrowser.


Dependencies
------------
The following, publicly-available Open Source softwares may be used as
part of the *Perftools* build process. Some of them are patched to support 
additional functionality. They are included with the distribution or if not
found, downloaded automatically from the Internet and compiled.

* [PAPI](http://icl.cs.utk.edu/papi/) (required)
* [Libmonitor](https://code.google.com/p/libmonitor/) (required)
* [Hpctoolkit](http://hpctoolkit.org/) (optional)
* [libunwind](http://www.nongnu.org/libunwind/) (optional)


Getting the tools
-----------------

      $ git clone https://github.com/tushar-mohan/perftools.git


Build Dependencies
------------------
`perftools` requires a number of operating system and build
utilities, such `make`, GNU-toolchain, `autoconf`, MPI, etc.

### Ubuntu/Debian

      $ sudo apt-get install build-essential gcc g++ gfortran libtool autoconf \
             cmake patch curl bzip2 environment-modules


Build the tools
---------------

  To build the minimal (PAPI and papiex) set of tools:
     
      $ ./build.sh

  To build the full set of tools, including HPCToolkit do:

      $ ./build.sh --all

The script honors `PREFIX` to set the install directory:

      $ ./build --all PREFIX=/opt/perftools-1.0.0

If your build environment is non-GNU, you may want to set
`CC`, `CXX`, `FORTRAN`, `MPICC`, `MPIF77`, `CC_OMPMPI` and `CC_OMP` 
on the command-line of the build script, like:

      $ ./build.sh --all CC=icc CXX=iCC FORTRAN=ifort

To point to a custom version of PAPI (not recommended):

      $ ./build.sh PAPI_PREFIX=/path/to/papi/install

      - or -

      $ ./build.sh PAPI_INC_PATH=/path/to/papi/headers PAPI_LIB_PATH=/path/to/papi/lib


You can also use `make` directly. Type `make help` for instructions.


Running the tools
-----------------

### Enable access to CPU counters
If you are using papiex/PAPI as a normal user then you will likely need to enable
collection of CPU counter data for ordinary users.

Check the existing value:

      $ cat /proc/sys/kernel/perf_event_paranoid
      1

You need a value of `-1`, `0` or `1`. `0` allows collection of CPU event data.
`1` also allows collection of kernel profiling data. `0` or `1` should suffice
for most needs. Do the following as `root`:

      # echo 0 > /proc/sys/kernel/perf_event_paranoid

To learn more about using the tools, see [papiex/README.md](papiex/README.md)


Testing the tools
-----------------

Make sure you have [enabled access to CPU counters](#enable-access-to-cpu-counters),
before running the commands below.

To quickly test the tools:

      $ make test

For complete coverage:

      $ make fulltest


Using the tools
---------------

Your tools install top-level directory contains a few scripts to help set up the environment.

For systems where `module` is in use:

    module load /path/to/perftools

For Bourne and C-shell users, perform one of the following:

    $ source /path/to/perftools.sh
    $ source /path/to/perftools.csh

Then you are free to run:

    $ papiex ...
    $ mpirun ... papiex ...


Documentation is available in the form of man pages and a Wiki here.


Platforms Tested
----------------

 * Haswell (Xeon(R) CPU E5-2698 v3), gcc 4.9.3
 * Ivy Bridge (Family: 6  Model: 58  Stepping: 9), gcc 5.4.0, Linux 4.10.0
 * POWER8E, gcc 4.8.5


Troubleshooting
---------------

* If papiex complains of error adding events, check that you have [enabled
  collection of CPU counters](#enable-access-to-cpu-counters) in the kernel.



License
-------
MIT

*Note* - During the build third-party software is downloaded that maybe
covered under separate licenses. Please refer to individual software
components, such as PAPI and HPCToolkit for their respective licenses.
