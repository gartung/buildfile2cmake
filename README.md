# buildfile2cmake

Creates CMake project files from SCRAM BuildFile.xml files.

## Usage

`cd` into a directory containing a SCRAM BuildFile.xml files.
Then run `PATH/TO/REPO/buildfile2cmake`.

The included builtin.json uses cmake variables defined by the cmake modlues in https://github.com/hsf/cmaketools as well as standard CMake modules.

The dictionaries branch has been tailored to only build the CMSSW ROOT dictionary libraries.

To use:

- Set up a scram project area
```
git clone -b dictionaries https://github.com/gartung/buildfile2cmake.git
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
-DORACLE_INCLUDE_DIR==/cvmfs/cms.cern.ch/slc7_amd64_gcc820/external/oracle/12.1.0.2.0-bcolbf/include
make VERBOSE=1 -j8
```
- Assuming the configuration and build complete you can now make the dictionaries available by setting LD_LIBRARY_PATH
```
LD_LIBRARY_PATH=$PWD/lib
```
