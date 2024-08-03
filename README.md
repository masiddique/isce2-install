# isce2-install
> Trying to simplify the installation of the ISCE2 framework

### Details of my machine
- OS: Linux (Ubuntu 24.04 LTS)
- Board: ASUS TUF GAMING X570-PRO WIFI II
- Processor: AMD Ryzen™ 9 5950X × 32
- Graphics: NVIDIA GeForce RTX™ 3090

### Step 1
Create a new conda environment, and install Python 3.9 in it.

`conda create --name isce python=3.9`
`conda activate isce`

### Step 2
Install packages listed in the `requirement.txt` file.

`conda install -c conda-forge --yes --file requirements.txt`

