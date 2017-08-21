# glibc-forward-compatibility-headers
Do you want to use the latest GCC for most optimized binary builds but also redistribute them without source on ancient LTS distributions? We have the solution!

The scenario is the following, you want to use the latest compiler and linux distro as a build machine and you have either of:
A) A commercial application or shared library for which you do not wish to release the source to be recompiled
B) An application or shared library, for which you do not wish to build a package for every single distribution imaginable
C) An open-source application, which you actually want to be successful and so require for the user to have to do absolutely zero config (recompilation is not an option) to get it running initially.

Born after 4 years of struggle and building "Build A World(TM)" on the oldest LTS release of Ubuntu/Debian, I've finally found a way to build on the latest OS with the latest GCC without sacrificing backwards compatibility.


Its a clever script found on stack overflow, which I've modified slightly to export all symbols (not just ones which are found to have versions higher than the threshold), which will generate inline-ASM C specifications which instruct the dynamic linker as to which versions of the GLIBC and GCC library functions your application wishes to retrieve.

Instructions for use:
1) Include the "gcc_libc_symvers.h" header in the top level .c or .cpp file in your application/library source (main.cpp for binaries) AND EVERY SINGLE SHARED LIBRARY YOU PROVIDE WITH IT!
2) Recompile all shared libraries that you provide with your application to make sure the correct symbol versions are picked up
3) Lastly Recompile your Apllication
Optional) Run "objdump -T" on all your shared libraries and application


So far I've included the scripts with hardcoded versions (GLIBC 2.19, GCC 4.8.0, etc.) which correspond to the lowest common denominator of glibc, libstdc++ and gcc shared library versions shipped with the oldest still supported distributions of Ubuntu (14.04 LTS), Fedora (25), OpenSUSE and Debian (Jessie).


Expect a new set of includes for Ubuntu 16.04 LTS in 2019!


# Advanced

To generate your own headers for your own lowest library required versions, alter the scripts and concatenate their outputs.
