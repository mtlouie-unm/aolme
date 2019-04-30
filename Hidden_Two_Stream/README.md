HTS (Hidden Two Stream)
## Obtaining HTS Singularity Image
The HTS Singulairty Image can be copied from the Xena-scratch directory. This can be done by doing:
```
mkdir <destination_directory>
rsync -aP /users/mlouie/xena-scratch/containers/hts_train_test.simg <path_to_destination_directory>
```
Note that this Singularity Image is ~12GB so copying may take a while.
## How to run HTS Singularity Image on UNM CARC (High Performance Computing)
### 1. Load Singularity Module
```
module load singularity3-3.0.3-gcc-4.8.5-3r534b5
```
### 2. Execute the Singularity Image
```
singularity shell -B <path_to_raw_videos>:<destination_directory_within_singularity_image> --writable --nv <path_to_hts_train_test.simg>
```
The destination directory within the Singularity Image could be ```/mnt```.
Note: The base directory of the HTS project folder is located ```/worksapce/Hidden-Two-Stream```, so to access this base directory run ```cd /workspace/Hidden-Two-Stream```.

### 3. Before any Training/Testing can be done, motion vectors needed to computed.
Follow the directions in the TSN README.md to generate the motion vectors.

### 4. Training
Training is done by executing two scripts: ```clean.sh``` and ```hts_prep.sh``` from the ```/workspace/Hidden-Two-Stream``` directory.
```
bash clean.sh
bash hts_prep.sh
bash fix.sh
bash hts_train.sh
```
### 5. Testing
Haven't done this yet. Training is taking forever.
