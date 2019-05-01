# TSN (Temporal Semgment Networks)
## Obtaining TSN Singularity Image
The TSN Singulairty Image can be copied from the Xena-scratch directory. This can be done by doing:
```
mkdir <destination_directory>
rsync -aP /users/mlouie/xena-scratch/containers/tsn_train_test.simg <path_to_destination_directory>
```
Note that this Singularity Image is ~12GB so copying may take a while.
## How to run TSN Singularity Image on UNM CARC (High Performance Computing)
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

### 3. Before any Training/Testing can be done, motion vectors are needed to be computed.
To compute the Motion Vectors for your video dataset you use the Denseflow executable that is included in the Singularity Image. This is done by running the follwing command from the ```/app``` directory.
```
bash scripts/extract_optical_flow.sh SRC_FOLDER OUT_FOLDER NUM_WORKER
```
- `SRC_FOLDER` points to the folder where you put the video dataset
- `OUT_FOLDER` points to the root folder where the extracted frames and optical images will be put in
- `NUM_WORKER` specifies the number of GPU to use in parallel for flow extraction, must be larger than 1

It is also necessary to modify the classInd.txt located at ```/app/data/ucf101_splits/``` to reflect your classes of your dataset.
The format of this file is as follows:
```
1 Writing
2 Typing
```

### 4. Training
Training is done by executing two scripts: ```clean.sh``` and ```tsn_train.sh``` from the ```/app``` directory.
It is important to specify the number of GPUs that will be utilized.

To change the number of iterations to train, you need to change the ```max_iter``` parameter located in the ```tsn_bn_inception_flow_solver.prototxt```. This solver.prototxt is located ```/app/models/ucf101/```.
```
bash clean.sh
bash tsn_train.sh <OUT_FOLDER> <num_gpus>
```
### 5. Testing
Sometimes when the Training Phase concludes (evident in the training_logs the last lines should say: "optimization done."), the terminal may hang. You can simply do ```ctrl+c```. Then proceed to run the testing script.
```
bash tsn_test.sh
```
## Using PBS Script to RUN TSN Singularity Image
``` 
qsub tsn_exec.pbs
```
