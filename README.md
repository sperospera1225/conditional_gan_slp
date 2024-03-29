# Implementing adversarial training based on Conditional GAN. 

The base code refers to the Source code for "Progressive Transformers for End-to-End Sign Language Production" (Ben Saunders, Necati Cihan Camgoz, Richard Bowden - ECCV 2020) 

<img width="1267" alt="image" src="https://user-images.githubusercontent.com/67995592/198539967-f1fd513b-743a-462b-8c15-1438646429dd.png">


# Usage

Install required packages using the requirements.txt file.

`pip install -r requirements.txt`

To run, start __main__.py with arguments "train" and ".\Configs\Base.yaml":

`python __main__.py train ./Configs/Base.yaml` 

An example train.log file can be found in ".\Configs\train.log" and a validation file at ".\Configs\validations.txt"

Back Translation model created from https://github.com/neccam/slt. Back Translation evaluation code coming soon.

# Data

Pre-processed Phoenix14T data can be requested via email at b.saunders@surrey.ac.uk. If you wish to create the data yourself, please follow below:

Phoenix14T data can be downloaded from https://www-i6.informatik.rwth-aachen.de/~koller/RWTH-PHOENIX-2014-T/ and skeleton joints can be extracted using OpenPose at  https://github.com/CMU-Perceptual-Computing-Lab/openpose and lifted to 3D using the 2D to 3D Inverse Kinematics code at https://github.com/gopeith/SignLanguageProcessing under 3DposeEstimator.

Prepare Phoenix14T (or other sign language dataset) data as .txt files for .skel, .gloss, .txt and .files. Data format should be parallel .txt files for "src", "trg" and "files", with each line representing a new sequence:

- The "src" file contains source sentences, with each line representing new sentence. 

- The "trg" file contains skeleton data of each frame, with a space separating frames. The joints should be divided by 3 to match the scaling I used. Each frame contains 150 joint values and a subsequent counter value, all separated by a space. Each sequence should be separated with a new line. If your data contains 150 joints per frame, please ensure that trg_size is set to 150 in the config file. 

- The "files" file should contain the name of each sequence on a new line. 

Examples can be found in /Data/tmp. Data path must be specified in config file.

# Pre-Trained Model

A pre-trained Progressive Transformer checkpoint can be downloaded from https://www.dropbox.com/s/l4xmnybp7luz0l3/PreTrained_PTSLP_Model.ckpt?dl=0. 

This model has a size of ```num_layers: 2```, ```num_heads: 4``` and ```embedding_dim: 512```, as outlined in ```./Configs/Base.yaml```. It has been pre-trained on the full PHOENIX14T dataset with the data format as above. The relevant train.log and validations.txt files can be found in ```.\Configs```.

To initialise a model from this checkpoint, pass the ```--ckpt ./PreTrained_PTSLP_Model.ckpt``` argument to either ```train``` or ```test``` modes. Additionally, to initialise the correct src_embed size, the config argument ```src_vocab: "./Configs/src_vocab.txt"``` must be set to the location of the src_vocab.txt, found under ```./Configs```. Please open an issue if this checkpoint cannot be downloaded or loaded.
