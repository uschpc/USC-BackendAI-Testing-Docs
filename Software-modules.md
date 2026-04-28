
On Backend.ai System, users can find and load software using the [Lmod](https://lmod.readthedocs.io/en/latest/) **module system**. Lmod dynamically changes your shell environment using **module files** to ensure the software applications and libraries you use are compatible. Module files are configuration files, written as Lua scripts, that instruct how to make an application or library available during your shell session. Typically, a module file contains instructions to initialize or modify environment variables, such as `PATH`.

Using a module system like Lmod is helpful because applications and libraries compiled with one compiler are not necessarily compatible with applications and libraries compiled with a different compiler, and your shell environment must be changed to accommodate these incompatibilities. **With Lmod, resetting your environment is done dynamically when you load new modules**. Loading a module will make available only compatible software for you to use. In this way, modules are organized in a hierarchy based on compilers.

### Finding software

When you log in, there is no modules loaded by default. You can load usc module first and then you can enter the `module list` command to view them:

```
work@main1[hYkzzAej-session]:~$ module load usc
work@main1[hYkzzAej-session]:~$ module list

Currently Loaded Modules:
  1) gcc/13.3.0      3) openblas/0.3.28   5) usc/13.3.0
  2) python/3.11.9   4) openmpi/5.0.5

```

This is our default software stack and the recommended one for most users, based on the GCC 13.3.0 compiler.

The naming convention for modules is `<software_name>/<version>` (e.g., `gcc/13.3.0`).

#### Using `module av`

To see what modules you can load into your environment, enter the command `module av`. With `gcc/13.3.0` loaded, this will print a large number of available modules:

```
work@main1[hYkzzAej-session]:~$: module av

---- /apps/spack/2406/apps/lmod/linux-rocky8-x86_64/openmpi/5.0.5-mufqd73/gcc/13.3.0 ----
   adiak/0.4.0                     hmmer/3.4                 parmetis/4.0.3         (D)
   armadillo/12.8.1                hpcg/3.1                  poppler/23.04.0
   arpack-ng/3.9.0                 hpl/2.3                   qmcpack/3.17.1
   arrow/7.0.0                     hypre/2.31.0              quantum-espresso/7.3.1
   boost/1.85.0                    ior/3.3.0                 scotch/7.0.3
   butterflypack/2.4.0             libcircle/0.3.0           sfcgal/1.5.1
   cgal/5.6                        lwgrp/1.0.5               silo/4.11.1
   charmpp/7.0.0                   mpigraph/main             slate/2023.11.05
   dtcmp/1.1.4                     namd/3.0b6                strumpack/7.2.0
   elpa/2023.11.001-patched        netcdf-c/4.9.2            superlu-dist/8.2.1
   fftw/3.3.10              (D)    netcdf-cxx4/4.3.1         thrift/0.18.1
   gdal/3.9.2                      netcdf-fortran/4.6.1      zoltan/3.901
   gmt/6.5.0                       netlib-scalapack/2.2.0
   hdf5/1.14.3              (D)    parallel-netcdf/1.12.3

----------------------------- /apps/lmod/modules/gcc/13.3.0 -----------------------------
   gromacs/2024.3

--------------- /apps/spack/2406/apps/lmod/linux-rocky8-x86_64/gcc/13.3.0 ---------------
   alsa-lib/1.2.12                           libtool/2.4.7
   ant/1.10.14                               libunistring/1.2
   aom/v3.9.1                                libunwind/1.6.2
   argobots/1.2                              libuv/1.46.0
   aria2/1.37.0                              libvorbis/1.3.7
...
```

To unload all your loaded modules, enter the command `module purge`. Then `module list` will return `No modules loaded`. If you enter `module av` again, then you will see only the core modules. These are primarily compilers but can also include applications like MATLAB that are pre-built and have no dependencies:

```
work@main1[hYkzzAej-session]:~$  module av

----------------------- /apps/spack/2406/apps/lmod/linux-rocky8-x86_64/openmpi/5.0.5-mufqd73/gcc/13.3.0 -----------------------
   adiak/0.4.0                     hmmer/3.4                 parmetis/4.0.3         (D)
   armadillo/12.8.1                hpcg/3.1                  poppler/23.04.0
   arpack-ng/3.9.0                 hpl/2.3                   qmcpack/3.17.1
   arrow/7.0.0                     hypre/2.31.0              quantum-espresso/7.3.1
   boost/1.85.0                    ior/3.3.0                 scotch/7.0.3
   butterflypack/2.4.0             libcircle/0.3.0           sfcgal/1.5.1
   cgal/5.6                        lwgrp/1.0.5               silo/4.11.1
   charmpp/7.0.0                   mpigraph/main             slate/2023.11.05
   dtcmp/1.1.4                     namd/3.0b6                strumpack/7.2.0
   elpa/2023.11.001-patched        netcdf-c/4.9.2            superlu-dist/8.2.1
   fftw/3.3.10              (D)    netcdf-cxx4/4.3.1         thrift/0.18.1
   gdal/3.9.2                      netcdf-fortran/4.6.1      zoltan/3.901
   gmt/6.5.0                       netlib-scalapack/2.2.0
   hdf5/1.14.3              (D)    parallel-netcdf/1.12.3

  Where:
   D:  Default Module

Use `module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
```

To explore the software stacks available, load one of these core modules and then see which new ones are unlocked. For example, to see all applications built with a certain compiler, load that compiler module and then enter `module av`.

#### Using `module spider`

If you know the name of a software package, use the `module spider` command to find out if it is available and how to load it.

For example, to search for Open MPI modules:

```
work@main1[hYkzzAej-session]:~$  module spider openmpi

-----------------------------------------------------------------------------------
  openmpi:
-----------------------------------------------------------------------------------
     Versions:
        openmpi/5.0.4
        openmpi/5.0.5

-----------------------------------------------------------------------------------
  For detailed information about a specific "openmpi" package (including how to load the modules) use the module's full name.
  Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider openmpi/5.0.5
-----------------------------------------------------------------------------------
```

This shows that there are two versions of Open MPI available. For more specific information, add the version to your command as given in the example:

```
work@main1[hYkzzAej-session]:~$  module spider openmpi/5.0.5

-----------------------------------------------------------------------------------
  openmpi: openmpi/5.0.5
-----------------------------------------------------------------------------------

    You will need to load all module(s) on any one of the lines below before the "openmpi/5.0.5" module is available to load.

      gcc/13.3.0
 
    Help:
      An open source Message Passing Interface implementation. The Open MPI
      Project is an open source Message Passing Interface implementation that
      is developed and maintained by a consortium of academic, research, and
      industry partners. Open MPI is therefore able to combine the expertise,
      technologies, and resources from all across the High Performance
      Computing community in order to build the best MPI library available.
      Open MPI offers advantages for system and software vendors, application
      developers and computer science researchers.
```




### Loading and unloading software

Typically, loading modules is as simple as entering `module load <software_name>`. The `<software_name>` must be visible when you run `module av`.

By entering:

```
module load <software_name>
```

Lmod will set your environment such that the software specified in `<software_name>` will be placed in your `PATH`, and then you can run the commands associated with `<software_name>`.

If there are multiple versions of `<software_name>`, you can specify a version:

```
module load <software_name>/<version>
```

For example, to load `gcc` version 12.3.0, enter:

```
module load gcc/12.3.0
```

If no version is specified:

```
module load gcc
```

then the default version will be loaded. The default version is indicated with a `(D)` next to it after running `module av`.

To unload a specific module, enter:

```
module unload <software_name>
```

To see the modules that you currently have loaded, enter:

```
module list
```

To see all modules that are compatible with the currently loaded modules and available to be loaded, enter:

```
module av
```

To unload all currently loaded modules and reset your environment, enter:

```
module purge
```

To reload the default `usc` module collection, enter:

```
module load usc
```

Enter `module help` to view more information about the available `module` commands.

### Environment management

One of the best features of Lmod is that it tracks software dependencies automatically. Importantly, there are two safety features when using the module system:

1. Only one version of a module can be loaded at once
2. Only one compiler and MPI implementation can be loaded at once

This ensures that the loaded modules are compatible with one another.

For example, let's say you want to use the `jellyfish` package compiled with `gcc/13.3.0`. You would load it with:

```
work@main1[hYkzzAej-session]:~$ module load gcc/13.3.0 jellyfish
```

If for some reason you need to switch to the `intel` compiler set, you can use the `module swap` command to swap out the `gcc` compiler:

```
work@main1[hYkzzAej-session]:~$ module swap gcc intel

Due to MODULEPATH changes, the following have been reloaded:
  1) jellyfish/2.3.0
```

Lmod automatically changes the `jellyfish` module to one that was compiled with `intel`.

The module system can also automatically replace or deactivate modules to ensure the packages that are loaded are compatible with each other. For example, switching from `gcc/13.3.0` to `gcc/12.3.0`:

```
work@main1[hYkzzAej-session]:~$ module load gcc/12.3.0

Inactive Modules:
  1) openblas/0.3.28     2) openmpi/5.0.5

Due to MODULEPATH changes, the following have been reloaded:
  1) python/3.11.9

The following have been reloaded with a version change:
  1) gcc/13.3.0 => gcc/12.3.0
```


```

### Additional resources

If you have questions about or need help with software modules, please [submit a help ticket](/user-support/submit-ticket) and we will assist you.

* [Lmod](https://lmod.readthedocs.io/en/latest/)
* [User Guide for Lmod](https://lmod.readthedocs.io/en/latest/010_user.html)
* [Advanced User Guide for Personal Modulefiles](https://lmod.readthedocs.io/en/latest/020_advanced.html)
