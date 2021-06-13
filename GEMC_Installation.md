# Simulation GEMC Installation

# ---------------------------------------------------------

# Install Docker Engine on Ubuntu

## Prerequisites

### OS requirements

To install Docker Engine, you need the 64-bit version of one of these Ubuntu
versions:

- Ubuntu Hirsute 21.04
- Ubuntu Groovy 20.10
- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)
- Ubuntu Xenial 16.04 (LTS)

Docker Engine is supported on `x86_64` (or `amd64`), `armhf`, and `arm64` architectures.

### Install using the repository

Before you install Docker Engine for the first time on a new host machine, you need
to set up the Docker repository. Afterward, you can install and update Docker
from the repository.

#### Set up the repository

1. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

   ```console
   sudo apt update && sudo apt upgrade
   ```
   ```console
   sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
   ```

3. Add Docker's official GPG key:

   ```console
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. Use the following command to set up the **stable** repository. To add the
   **nightly** or **test** repository, add the word `nightly` or `test` (or both)
   after the word `stable` in the commands below.

   ```console
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

#### Install Docker Engine

1. Update the `apt` package index, and install the _latest version_ of Docker
   Engine and containerd, or go to the next step to install a specific version:

   ```console
   sudo apt update
   ```
   ```console
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

2. Verify that Docker Engine is installed correctly by running the `hello-world`
   image.

   ```console
   sudo docker run hello-world
   ```

This command downloads a test image and runs it in a container. When the
container runs, it prints an informational message and exits.

Docker Engine is installed and running. The `docker` group is created but no users
are added to it. You need to use `sudo` to run Docker commands.


# Download `clas12software` docker

Create folder : `mkdir ~/mywork` and `cd ~/mywork`

   ```console 
   sudo apt install python-is-python3 maven groovy
   ```

   ```console 
   git clone https://github.com/gemc/detectors 
   ```
   
   ```console 
   cd detectors/clas12
   ```
   ```console 
   git clone https://github.com/JeffersonLab/clas12-offline-software
   ```
   ```console 
   cd clas12-offline-software
   ```
   ```console 
   git checkout Alert
   ```
   ```console 
   ./build-coatjava.sh
   ```
   ```console 
   cp coatjava/lib/clas/* ..
   ```
   ```console 
   cd .. 
   ```
   ```console 
   ./clas12-offline-software/coatjava/bin/run-groovy alert/AHDC_geom/factory_ahdc.groovy --variation rga_fall2018 --runnumber 11
   ```
   ```console 
   cp ahdc__* alert/AHDC_geom/
   ```
   ```console 
   ./clas12-offline-software/coatjava/bin/run-groovy alert/ATOF_geom/factory_atof.groovy --variation rga_fall2018 --runnumber 11
   ```
   ```console 
   cp atof__* alert/ATOF_geom/
   ```
   ```console 
   cd
   ```
   ```console
   git clone https://github.com/gemc/source ~/mywork/source
   git clone https://github.com/Viktoriya89/source ~/mywork/source
   ```   
   ```console
   sudo docker run -it --rm -v ~/mywork:/jlab/work/mywork jeffersonlab/gemc:devel bash
   ```
   ```console
   cd /jlab/work/mywork/detectors/clas12/alert/AHDC_geom/
   ```
   ```console
   ./ahdc.pl config.dat
   ```   
   ```console
   cd /jlab/work/mywork/detectors/clas12/alert/ATOF_geom/  
   ```
   Change `config.dat` line `detector_name : myatof ` to `detector_name : atof ` 
   
   ```console
   ./atof.pl config.dat
   ```   
   
   
   ```console
   cd /jlab/work/mywork/source 
   ```   
   
   build gemc : 
   ```console
   scons -j4 OPT=1
   ```   
   Create the gcard based on the existant one : 
   





















Download source and detectors from Viktoriya github :
   ```console
   git clone https://github.com/Viktoriya89/source ~/mywork/source
   ```
   ```console
   git clone https://github.com/Viktoriya89/detectors ~/mywork/detectors
   ```

   ```console
   sudo docker run -it --rm -v ~/mywork:/jlab/work/mywork jeffersonlab/gemc:devel bash
   ```

   Build gemc from source : `cd ~/mywork/source`
   ```console
   scons -j4 OPT=1
   ```

   For having an interactive windows :

   ```console
   sudo docker run -it --rm -p 6080:6080 -v ~/mywork:/jlab/work/mywork jeffersonlab/gemc:devel
   ```

   For quit interactive docker to : `crtl p + crtl q`
