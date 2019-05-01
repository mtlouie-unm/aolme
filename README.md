# Methods for AOLME Dataset
## Singularity Scripts (Optional but make things a lot easier)
The following will make using Singularity a lot simpler. Also the following will be useful for all the methods since they all use Singularity.
### 1. Download the bash_scripts folder from this repo to your directory on a UNM CARC machine (i.e. xena)
### 2. Add the following lines of code to your ~/.bashrc file.
Note that the following commands being added to your .bashrc file means that singularity and python3.6 will be loaded in automatically every time a new shell is opened. Also note that it is not required to load python3.6 since python2.7 is loaded in by default but some python scripts may require python3.6 so it's best to load it in just in case.
```
# Open the .bashrc file
vim ~/.bashrc
```
```
# Add the following three lines somewhere in the .bashrc file
module load singularity3-3.0.3-gcc-4.8.5-3r534b5
module load python/3.6.0

export PATH="$PATH:~/bash_scripts/"
```
```
# Save and exit the .bashrc file and either reconnect your ssh session
# Or run the following command to save your changes to the current shell
source ~/.bashrc
```
### 3. Now you can run the bash scripts from anywhere
#### **build script**
Pull a docker image from DockerHub and convert it to a singularity image. Note that this could take a while to build.
```
build <name of image>.simg <docker hub path>

# To download the the resnets image for example you would:
build resnets.simg docker://akash1196/resnets:latest
```
#### **shell script**
Shell into a singularity image. This command will mount the current directory onto the /mnt folder in the image by default. This script should also allow you to read/write within the image.
```
# If path to mound directory isn't specefied '.' will be used by default
shell <path to singularity image> <path to mount directory>

# To shell into the resnets image for example you would:
shell ~/singularity_images/resnets.simg

# To mount, say a directory of videos onto the image:
shell ~/singularity_images/resnets.simg  ~/videos
```
#### **pretty script**
Some of the results in the resnets method are written to JSON. This is a simple script to make the JSON files more legible.
```
# The formatted JSON file will be saved to <name of file>.pretty
pretty <path to JSON file>

# This will reformat the test.json file for example and save it to test.json.pretty
pretty results/test.json
```
