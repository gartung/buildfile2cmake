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
-DCMAKE_PREFIX_PATH="/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fmt/7.0.1;/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/clhep/2.4.1.3-ghbfee" \
-DBoost_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/boost/1.72.0-gchjei/include \
-DROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/lcg/root/6.20.06-ghbfee6/cmake \
-DCMakeTools_DIR=../src/cmaketools \
-DROOT_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/lcg/root/6.20.06-ghbfee6/include \
-DTBB_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tbb/2020_U2-ghbfee \
-DTINYXML2_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tinyxml2/6.2.0-ghbfee \
-DHEPMC_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/hepmc/2.06.07-bcolbf2  \
-DXERCESC_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/xerces-c/3.1.3-bcolbf2 \
-DFMT_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fmt/7.0.1/include \
-DCUDA_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/cuda/11.1.0/include \
-DEIGEN_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/eigen/d812f411c3f9-ghbfee/include \
-DFASTJET_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fastjet/3.3.4/include \
-DCLASSLIB_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/classlib/3.1.3-ghbfee/include  \
-DMESCHACH_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/meschach/1.2.pCMS1-bcolbf2/include \
-DVDT_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/cms/vdt/0.4.0-ghbfee \
-DHEPPDT_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/heppdt/3.03.00-ghbfee2 \
-DMKFIT_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/mkfit/2.0.1-ghbfee2/include \
-DPYBIND11_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/py2-pybind11/2.4.3-bcolbf2/include/python2.7 \
-DMD5ROOT=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/md5/1.0.0-bcolbf2 \
-DCPPUNITROOT=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/cppunit/1.15.x-bcolbf2 \
-DCLHEP_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/clhep/2.4.1.3-ghbfee
make VERBOSE=1 -j8
```
- Assuming the configuration and build complete you can now make the dictionaries available by setting LD_LIBRARY_PATH
```
LD_LIBRARY_PATH=$PWD/lib:$LD_LIBRARY_PATH
```

- The cmake command will produce warnings like those shown below. These are safe to ignore as long as the Makefile is generated.
```
-- The C compiler identification is GNU 8.4.0

-- The CXX compiler identification is GNU 8.4.0
-- Check for working C compiler: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/gcc/8.2.0-bcolbf/bin/cc
-- Check for working C compiler: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/gcc/8.2.0-bcolbf/bin/cc - works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/gcc/8.2.0-bcolbf/bin/c++
-- Check for working CXX compiler: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/gcc/8.2.0-bcolbf/bin/c++ - works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
'setBUILDTEST=Truetoruntests'
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Found Boost: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/boost/1.72.0-gchjei/include (found suitable version "1.72.0", minimum required is "1.67.0") found components: filesystem thread iostreams regex serialization system program_options python27 chrono date_time missing components: atomic
-- Found Python2: /usr/lib64/libpython2.7.so (found version "2.7") found components: Development 
-- CLHEP version: 2.4.1.3
-- Could NOT find CLHEP (missing: CLHEP_LIBRARIES) 
CMake Warning (dev) at /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/cmake/3.17.2-pafccj/share/cmake-3.17/Modules/FindPackageHandleStandardArgs.cmake:272 (message):
  The package name passed to `find_package_handle_standard_args` (MD5) does
  not match the name of the calling package (CMSMD5).  This can lead to
  problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  cmaketools/modules/FindCMSMD5.cmake:23 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  CMakeLists.txt:56 (find_package)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found MD5: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/md5/1.0.0-bcolbf2/include  
-- Could NOT find CppUnit (missing: CPPUNIT_LIBRARY) 
CMake Warning at CMakeLists.txt:60 (find_package):
  By not providing "FindGeant4.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "Geant4", but
  CMake did not find one.

  Could not find a package configuration file provided by "Geant4" with any
  of the following names:

    Geant4Config.cmake
    geant4-config.cmake

  Add the installation prefix of "Geant4" to CMAKE_PREFIX_PATH or set
  "Geant4_DIR" to a directory containing one of the above files.  If "Geant4"
  provides a separate development package or SDK, be sure it has been
  installed.


-- Found HepMC: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/hepmc/2.06.07-bcolbf2/include  
-- Found HepPDT: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/heppdt/3.03.00-ghbfee2/include  
-- Found OpenGL: /usr/lib64/libOpenGL.so   
-- Found OpenSSL: /usr/lib64/libcrypto.so (found version "1.0.2k")  
-- Found SIGCPP: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/sigcpp/2.6.2-bcolbf2/include/sigc++-2.0  
CMake Warning (dev) at cmaketools/modules/FindTBB.cmake:108 (SET):
  implicitly converting 'DOC' to 'STRING' type.
Call Stack (most recent call first):
  CMakeLists.txt:72 (find_package)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found TBB: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tbb/2020_U2-ghbfee (found version "2020.2")  
-- Found TINYXML2: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tinyxml2/6.2.0-ghbfee/include  
-- Found UUID: /usr/include  
-- Found XercesC: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/xerces-c/3.1.3-bcolbf2/include  
-- Found Xrootd: /usr/include  
-- Configuring done
-- Generating done
-- Build files have been written to: /scratch/gartung/CMSSW_11_2_0_pre7/build

```
