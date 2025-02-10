# Building & Installing ISCE2 on a linux machine using CMake
> Trying to simplify the installation of the ISCE2 framework (primarily for myself)

Inspiration: [https://github.com/lijun99/isce2-install] (Lijun Zhu)

### Details of my machine
- OS: Linux (Ubuntu 24.04 LTS)
- Board: ASUS TUF GAMING X570-PRO WIFI II
- Processor: AMD Ryzen™ 9 5950X × 32
- Graphics: NVIDIA GeForce RTX™ 3090

### Step 0 [Optional]
Open the shell on your machine, and switch to your work directory. I recommend that you setup a folder, say "ISCE". The ISCE2 framework that you'll install is available here: https://github.com/isce-framework/isce2. Let's say you want to install the latest release. I recommend [optionally]:

```
mkdir ISCE && cd ISCE
mkdir v2_6_3 # folder named according to the release, assuming the latest is 2.6.3
cd v2_6_3
```
If you intend to use `mdx` visualization tool, please install beforehand, as follows:

```
sudo apt update
sudo apt install libmotif-dev
```

### Step 1
If you don't have conda installed on your machine, please do it first. Next, create a new conda environment, and install Python in it.

```
conda create --name isce python=3.11
conda activate isce
```

### Step 2
Install packages listed in the `requirement.txt` file (provided in this repository).

```
conda install -c conda-forge --file requirements.txt
```

### Step 3
Check out the latest ISCE2 from its GitHub repository. The code will be checked out at the current directory.

```
git clone https://github.com/isce-framework/isce2.git
cd isce2
```

Now make a subfolder for the build
```
mkdir build
cd build
```

Create a symbolic link in this folder to point to the site packages directory.
```
ln -sf `python3 -c 'import site; print(site.getsitepackages()[0])'` $CONDA_PREFIX/packages
```
Here, `python3 -c 'import site; print(site.getsitepackages()[0])` is simply going to return the complete path where Python is installed in the current conda environment. In my case, it is `/home/adnan/miniconda3/envs/isce/lib/python3.11/site-packages`.

### Step 4
Configure Cmake and install

```
# run cmake config
cmake .. -DCMAKE_INSTALL_PREFIX=$CONDA_PREFIX \
  -DCMAKE_CUDA_ARCHITECTURES=native \
  -DCMAKE_PREFIX_PATH=${CONDA_PREFIX} \
  -DCMAKE_BUILD_TYPE=Release 
```
Carefully observe the screen dump. There are likely to be several warnings. If there are any missing libraries, try to troubleshoot. For example, if you're lacking a Fortran compiler, you may see this error:

```
CMake Error at CMakeLists.txt:3 (project)
  No CMAKE_Fortran_COMPILER could be found.
```
In such a case, install the requisite compiler, and start again with step 4.

If there is no critical error, proceeded as follows:

```
# compile and install 
make -j && make install
```

### Step 5
Check if the installation is correct:

```
python3 -c 'import isce'
```
This should print something like `Using default ISCE Path: /home/adnan/miniconda3/envs/isce/lib/python3.11/site-packages/isce`.

```
topsApp.py --help --steps
```
It should print details:

```
2024-08-04 11:37:09,034 - isce.insar - INFO - ISCE VERSION = 2.6.3, RELEASE_SVN_REVISION = ,RELEASE_DATE = 20230418, CURRENT_SVN_REVISION = 
ISCE VERSION = 2.6.3, RELEASE_SVN_REVISION = ,RELEASE_DATE = 20230418, CURRENT_SVN_REVISION = 
None
The currently supported sensors are:  ['SENTINEL1']

...
```

Things should work now!

## Recommendations
If you're interested in using MintPy with ISCE2, it is [recommended](https://groups.google.com/g/mintpy/c/GNVrSEZLo00/m/re5wpEziAAAJ) that you install both in the same conda environment.

