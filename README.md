# isce2-install
> Trying to simplify the installation of the ISCE2 framework

Inspiration: [https://github.com/lijun99/isce2-install] (Lijun Zhu)

### Details of my machine
- OS: Linux (Ubuntu 24.04 LTS)
- Board: ASUS TUF GAMING X570-PRO WIFI II
- Processor: AMD Ryzen™ 9 5950X × 32
- Graphics: NVIDIA GeForce RTX™ 3090

### Step 1
Open the shell on your machine. If you don't have conda installed on your machine, please do it first. Next, create a new conda environment, and install Python 3.9 in it.

```
conda create --name isce python=3.11
conda activate isce
```

### Step 2
Install packages listed in the `requirement.txt` file.

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
Carefully observe the screen dump. There are likely to be several warnings. If there are any missing libraries, try to troubleshoot. In my case, I noticed the following:

```
-- Could NOT find Motif (missing: MOTIF_LIBRARIES MOTIF_INCLUDE_DIR) 
-- Could NOT find X11 (missing: Xt) 
```

I checked if my system had `Xt` installed:

`dpkg -l | grep libxt` showed me the relevant files, but `dpkg -l | grep libxt-dev` showed nothing. So I did the following next.

```
sudo apt-get update
sudo apt-get install libxt-dev
```
Then I also installed Motif: `sudo apt install libmotif-dev`, and configured Cmake again. 

There were still several warnings, related mostly to the library `[libgomp.so.1]` which is a critical library for programs using OpenMP for parallel processing. I probably I'll need to fix this issue, but for now I proceeded as follows:

```
# compile and install 
make -j && make install
```
Now I noticed that 

