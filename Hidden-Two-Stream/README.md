HTS (Hidden Two Stream)
## Obtaining HTS Singularity Image
The HTS Singulairty Image can be built from DockerHub. This can be done by doing:
```
singularity build --sandbox <new_singularity_image_name> docker://mlouieunm/aolme:Hidden-Two-Stream
```

## How to run HTS Singularity Image on UNM CARC (High Performance Computing)
### 1. Load Singularity Module
```
module load singularity3-3.0.3-gcc-4.8.5-3r534b5
```
### 2. Execute the Singularity Image
```
singularity shell -B <path_to_raw_videos>:<destination_directory_within_singularity_image> --writable --nv <path_to_hts.simg>
```
The destination directory within the Singularity Image could be ```/mnt```.
Note: The base directory of the HTS project folder is located ```/worksapce/Hidden-Two-Stream```, so to access this base directory run ```cd /workspace/Hidden-Two-Stream```.

### 3. Before any Training/Testing can be done, motion vectors are needed to be computed.
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
To test run the python file: ```demo_hidden.py``` located at ```/workspace/Hidden-Two-Stream/models/ucf101_split1_unsup_end/evalucf101/```. Before running this python file you need to change the paths to the trained model. Within the python file change the following variable defintions:
```model_file = '../logs_end/vgg16_end_iter_<number_of_iterations>.caffemodel'```

Now run the ```hts_test.sh``` located at ```/workspace/Hidden-Two-Stream```
```
bash hts_test.sh
```
