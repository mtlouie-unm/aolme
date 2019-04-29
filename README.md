# TSN (Temporal Semgment Networks)
## Obtaining TSN Singularity Image
The TSN Singulairty Image can be copied from the Xena-scratch directory. This can be done by doing:
```
mkdir <destination_directory>
rsync -aP /users/mlouie/xena-scratch/containers/tsn_train_test.simg <path_to_destination_directory>
```
Note that this Singularity Image is ~12GB so copying may take a while.
## How to run TSN Singularity image on UNM CARC (High Performance Computing)
### 1. Load Singularity Module
```
module load singularity3-3.0.3-gcc-4.8.5-3r534b5
```
### 2. Execute the Singularity Image
```
singularity shell -B <path_to_raw_videos>:<destination_directory_within_singularity_image> --writable --nv <path_to_tsn_train_test.simg>
```
The destination directory within the Singularity Image could be ```/mnt```.
Note: The base directory of the TSN project folder is located ```/app```, so to access this base directory run ```cd /app```.

### 3. Before any Training/Testing can be done, motion vectors needed to computed.
To compute the Motion Vectors for your video dataset you use the Denseflow executable that is included in the Singularity Image. This is done by running the follwing command from the ```/app``` directory.
```bash scripts/extract_optical_flow.sh SRC_FOLDER OUT_FOLDER NUM_WORKER
```
- `SRC_FOLDER` points to the folder where you put the video dataset
- `OUT_FOLDER` points to the root folder where the extracted frames and optical images will be put in
- `NUM_WORKER` specifies the number of GPU to use in parallel for flow extraction, must be larger than 1
