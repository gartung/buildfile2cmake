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
cmake ../src  -Wno-dev \
-DCMAKE_PREFIX_PATH="/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fmt/7.0.1;/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/clhep/2.4.1.3-ghbfee" \
-DBoost_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/boost/1.72.0-gchjei/include \
-DROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/lcg/root/6.20.06-ghbfee6/cmake \
-DCMakeTools_DIR=../src/cmaketools \
-DTBB_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tbb/2020_U2-ghbfee \
-DTINYXML2_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tinyxml2/6.2.0-ghbfee \
-DHEPMC_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/hepmc/2.06.07-bcolbf2  \
-DFMT_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/fmt/7.0.1/include \
-DEIGEN_INCLUDE_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/eigen/d812f411c3f9-ghbfee/include \
-DMD5ROOT=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/md5/1.0.0-bcolbf2 \
-DCLHEP_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/clhep/2.4.1.3-ghbfee \
-DUUID_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/libuuid/2.34-bcolbf2 \
-DVDT_ROOT_DIR=/cvmfs/cms.cern.ch/slc7_amd64_gcc820/cms/vdt/0.4.0-ghbfee
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
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Found Boost: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/boost/1.72.0-gchjei/include (found suitable version "1.72.0", minimum required is "1.67.0") found components: filesystem thread iostreams regex serialization system program_options python27 chrono date_time missing components: atomic
-- Found Python2: /usr/lib64/libpython2.7.so (found version "2.7") found components: Development 
-- CLHEP version: 2.4.1.3
-- Could NOT find CLHEP (missing: CLHEP_LIBRARIES) 
-- Found MD5: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/md5/1.0.0-bcolbf2/include  
-- Found HepMC: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/hepmc/2.06.07-bcolbf2/include  
-- Found TBB: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tbb/2020_U2-ghbfee (found version "2020.2")  
-- Found TINYXML2: /cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/tinyxml2/6.2.0-ghbfee/include  
-- Configuring done
-- Generating done
-- Build files have been written to: /scratch/gartung/CMSSW_11_2_0_pre7/build
```
