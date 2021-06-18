```console
xhost 127.0.0.1
```
```console
xhost local:root
```

```console
export DISPLAY=:0
```

```console
docker run -it --rm -v /cvmfs:/cvmfs -v /tmp/.X11-unix:/tmp X11-unix -v /vol0/mywork:/jlab/work/mywork -e DISPLAY=$DISPLAY jeffersonlab/clas12software:production /bin/bash
```

Nouvelle gcard : 
 
```vim
<gcard>

	<!--ALERT -->
	<detector name="/jlab/work/mywork/ahdc" factory="TEXT" variation="default"/>
	<detector name="/jlab/work/mywork/myatof" factory="TEXT" variation="default"/>

	<!-- magnets -->
	<detector name="experiments/clas12/magnets/solenoid" factory="TEXT" variation="original"/>
	<detector name="experiments/clas12/magnets/cad/"     factory="CAD" />

	<!--high threshold cherenkov -->
	<detector name="experiments/clas12/htcc/htcc"      factory="TEXT" variation="original"/>

	<!-- forward carriage -->
	<detector name="experiments/clas12/fc/forwardCarriage" factory="TEXT" variation="TorusSymmetric"/>

	<detector name="experiments/clas12/dc/dc"              factory="TEXT" variation="default"/>
	<detector name="experiments/clas12/ftof/ftof"          factory="TEXT" variation="default"/>
	<detector name="experiments/clas12/ec/ec"              factory="TEXT" variation="default"/>
	<detector name="experiments/clas12/pcal/pcal"          factory="TEXT" variation="default"/>
	<detector name="experiments/clas12/ltcc/ltcc"          factory="TEXT" variation="default"/>
	<detector name="experiments/clas12/ltcc/cad_cone/"     factory="CAD"/>
	<detector name="experiments/clas12/ltcc/cad/"          factory="CAD"/>

	<!-- you can scale the fields here. Remember torus -1 means e- INBENDING  -->
	<option name="SCALE_FIELD" value="TorusSymmetric, -1"/>
	<option name="SCALE_FIELD" value="clas12-newSolenoid, -1"/>

	<!-- hall field  -->
	<option name="HALL_FIELD"  value="clas12-newSolenoid"/>

	<!-- fields, precise mode -->
	<option name="FIELD_PROPERTIES" value="TorusSymmetric,     2*mm, G4ClassicalRK4, linear"/>
	<option name="FIELD_PROPERTIES" value="clas12-newSolenoid, 1*mm, G4ClassicalRK4, linear"/>

	<!-- beam conditions -->
	<option name="BEAM_P"   value="e-, 10.6*GeV, 0.0*deg, 0*deg"/>
	<option name="BEAM_V"    value="(0, 0, 0)cm"/>
	<option name="SPREAD_V"  value="(0.0, 2.5)cm"/>

	<option name="SAVE_ALL_MOTHERS" value="0"/>
	<option name="RECORD_OPTICALPHOTONS"   value="1"/>

	<option name="PHYSICS" value="FTFP_BERT + STD + Optical"/>

	<option name="OUTPUT"   value="txt, out.txt"/>

	<!--  Will print message every 10 events -->
	<option name="PRINT_EVENT"    value="1000" />

	<!--  Do not track staff after the apex -->
	<option name="MAX_Z_POS" value="8000"/>


</gcard>
```





























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
   ```   
   ```console
   git clone --recurse-submodules https://github.com/gavalian/hipo
   ``` 
   ```console
   cd hipo
   ``` 
   ```console
   make
   ``` 
   ```console
   cp -r ~/mywork/hipo/hipo4 ~/mywork/source/output/
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
