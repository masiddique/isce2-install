# isce2-install
> Trying to simplify the installation of the ISCE2 framework

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

`conda install -c conda-forge --file requirements.txt`

### Step 3
Check out the latest ISCE2 from its GitHub repository. The code will be checked out at the current directory.

` git clone https://github.com/isce-framework/isce2.git`



