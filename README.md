# buildfile2cmake

Creates CMake project files from SCRAM BuildFile.xml files for dictionary generation and compilation.

## Usage

The included builtin.json uses cmake variables defined by the cmake modlues in https://github.com/hsf/cmaketools as well as standard CMake modules.

The dictionaries branch has been tailored to only build the CMSSW ROOT dictionary libraries.

To use:

- Clone the dictionaries branch of the repo
```
git clone -b dictionaries https://github.com/gartung/buildfile2cmake.git
```

- Set up a scram project area
```
scram pro CMSSW CMSSW_11_2_0_pre7
```
- Checkout all CMSSW packages
```
cd CMSSW_11_2_0_pre7/src
eval `scram runtime -sh`
git-cms-add-pkg '*/*' 
PATH/TO/REPO/buildfile2cmake
```
- Clone the cmaketools repo
```
git clone https://github.com/gartung/cmaketools.git
```
- Make a build directory and run CMake with the defines needed to find the header files used in dictionary generation
```
cd ..
mkdir build
cd build
source /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/gcc/8.2.0-bcolbf/etc/profile.d/init.sh 
source /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/cmake/3.17.2-pafccj/etc/profile.d/init.sh
cmake ../src  \
-DBoost_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/boost/1.72.0-gchjei/include \
-DROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/lcg/root/6.20.06-ghbfee6/cmake \
-DCMakeTools_DIR=../src/cmaketools \
-DROOT_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/lcg/root/6.20.06-ghbfee6/include \
-DMD5_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/md5/1.0.0-bcolbf2/include \
-DTBB_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tbb/2020_U2-ghbfee/include  \
-DTINYXML2_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tinyxml2/6.2.0-ghbfee/include \
-DSIGCPP_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/sigcpp/2.6.2-bcolbf2/include/sigc++-2.0 \
-DHEPMC_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/hepmc/2.06.07-bcolbf2/include  \
-DXERCESC_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/xerces-c/3.1.3-bcolbf2/include \
-DFMT_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fmt/7.0.1/include \
-DCUDA_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/cuda/11.1.0/include \
-DEIGEN_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/eigen/d812f411c3f9-ghbfee/include \
-DFASTJET_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fastjet/3.3.4/include \
-DCLASSLIB_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/classlib/3.1.3-ghbfee/include  \
-DORACLE_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/oracle/12.1.0.2.0-bcolbf/include
make VERBOSE=1 -j8
```
- Assuming the configuration and build complete you can now make the dictionaries available by setting LD_LIBRARY_PATH
```
LD_LIBRARY_PATH=$PWD/lib
```

- The cmake command will produce errors like those shown below. These are safe to ignore as local as the Makefile is generated. These errors are reported because the package libraries and tests may need those dependencies but the dictionary libraries do not.
```
-- Could NOT find CLHEP (missing: CLHEP_INCLUDE_DIR CLHEP_LIBRARIES) 
CMake Warning (dev) at /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/cmake/3.17.2-pafccj/share/cmake-3.17/Modules/FindPackageHandleStandardArgs.cmake:272 (message):
  The package name passed to `find_package_handle_standard_args` (MD5) does
  not match the name of the calling package (CMSMD5).  This can lead to
  problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  cmaketools/modules/FindCMSMD5.cmake:23 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  CMakeLists.txt:55 (find_package)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Could NOT find MD5 (missing: MD5_LIBRARIES) 
-- Could NOT find CppUnit (missing: CPPUNIT_INCLUDE_DIR CPPUNIT_LIBRARY) 
CMake Warning at CMakeLists.txt:59 (find_package):
  By not providing "FindDAVIX.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "DAVIX", but
  CMake did not find one.

  Could not find a package configuration file provided by "DAVIX" with any of
  the following names:

    DAVIXConfig.cmake
    davix-config.cmake

  Add the installation prefix of "DAVIX" to CMAKE_PREFIX_PATH or set
  "DAVIX_DIR" to a directory containing one of the above files.  If "DAVIX"
  provides a separate development package or SDK, be sure it has been
  installed.


-- Could NOT find XercesC (missing: XERCESC_LIBRARY) (Required is at least version "3.1.3")
CMake Warning at CMakeLists.txt:61 (find_package):
  Found package configuration file:

    /cvmfs/cms.cern.ch/slc7_amd64_gcc820/cms/cmssw/CMSSW_11_2_0_pre7/external/slc7_amd64_gcc820/lib/Geant4-10.6.1/Geant4Config.cmake

  but it set Geant4_FOUND to FALSE so package "Geant4" is considered to be
  NOT FOUND.  Reason given by package:

  Geant4 could not be found because dependency XercesC could not be found.



-- Could NOT find HepMC (missing: HEPMC_LIBRARIES) 
-- Could NOT find HepPDT (missing: HEPPDT_INCLUDE_DIR HEPPDT_LIBRARIES) 
-- Could NOT find SIGCPP (missing: SIGCPP_LIBRARIES) 
-- Could NOT find TINYXML2 (missing: TINYXML2_LIBRARIES) 
-- Could NOT find XercesC (missing: XERCESC_LIBRARY) 
CMake Warning at CMakeLists.txt:83 (find_package):
  By not providing "Findbenchmark.cmake" in CMAKE_MODULE_PATH this project
  has asked CMake to find a package configuration file provided by
  "benchmark", but CMake did not find one.

  Could not find a package configuration file provided by "benchmark" with
  any of the following names:

    benchmarkConfig.cmake
    benchmark-config.cmake

  Add the installation prefix of "benchmark" to CMAKE_PREFIX_PATH or set
  "benchmark_DIR" to a directory containing one of the above files.  If
  "benchmark" provides a separate development package or SDK, be sure it has
  been installed.


CMake Warning at CMakeLists.txt:85 (find_package):
  By not providing "Findpybind11.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "pybind11",
  but CMake did not find one.

  Could not find a package configuration file provided by "pybind11" with any
  of the following names:

    pybind11Config.cmake
    pybind11-config.cmake

  Add the installation prefix of "pybind11" to CMAKE_PREFIX_PATH or set
  "pybind11_DIR" to a directory containing one of the above files.  If
  "pybind11" provides a separate development package or SDK, be sure it has
  been installed.


-- Configuring done
-- Generating done
-- Build files have been written to: /scratch/gartung/CMSSW_11_2_0_pre7/build
```
