# Kaggle-Human-Protein-Atlas-Image-Classification
Classify subcellular protein patterns in human cells

In this competition, Kagglers will develop models capable of classifying mixed patterns of proteins in microscope images. The Human Protein Atlas will use these models to build a tool integrated with their smart-microscopy system to identify a protein's location(s) from a high-throughput image.

Proteins are “the doers” in the human cell, executing many functions that together enable life. Historically, classification of proteins has been limited to single patterns in one or a few cell types, but in order to fully understand the complexity of the human cell, models must classify mixed patterns across a range of different human cells.

Images visualizing proteins in cells are commonly used for biomedical research, and these cells could hold the key for the next breakthrough in medicine. However, thanks to advances in high-throughput microscopy, these images are generated at a far greater pace than what can be manually evaluated. Therefore, the need is greater than ever for automating biomedical image analysis to accelerate the understanding of human cells and disease.

Nature Methods has indicated interest in considering a paper discussing the outcome and approaches of the challenge. The Human Protein Atlas team would like to invite top performing teams to join as co-authors in the writing of this paper.

Top performing teams will also be eligible to compete for the special prize. Additional information for both the special prize and co-authoring for Nature Methods will become available through the Discussion posts once the main competition is complete.

## Acknowledgements
The Human Protein Atlas is a Sweden-based initiative aimed at mapping all human proteins in cells, tissues and organs. All the data in the knowledge resource is open access to allow anyone to pursue exploration of the human proteome. In a recent publication, the Human Protein Atlas team has demonstrated the promise of both citizen science and artificial intelligence approaches in describing the location of human proteins in images, however current results are yet to approach expert-level annotations (Sullivan et al, Nature Biotechnology, Oct 2018).


## Submissions

Submissions will be evaluated based on their macro F1 score.

Submission File
For each Id in the test set, you must predict a class for the Target variable as described in the data page. Note that multiple labels can be predicted for each sample.

The file should contain a header and have the following format:

Id,Predicted  
00008af0-bad0-11e8-b2b8-ac1f6b6435d0,0 1  
0000a892-bacf-11e8-b2b8-ac1f6b6435d0,2 3
0006faa6-bac7-11e8-b2b7-ac1f6b6435d0,0  
0008baca-bad7-11e8-b2b9-ac1f6b6435d0,0  
000cce7e-bad4-11e8-b2b8-ac1f6b6435d0,0  
00109f6a-bac8-11e8-b2b7-ac1f6b6435d0,1 28  
...


## Timeline

January 3, 2019 - Entry deadline. You must accept the competition rules before this date in order to compete.

January 3, 2019 - Team Merger deadline. This is the last day participants may join or merge teams.

January 10, 2019 - Final submission deadline.

All deadlines are at 11:59 PM UTC on the corresponding day unless otherwise noted. The competition organizers reserve the right to update the contest timeline if they deem it necessary.



## Reproducibility

Below you can find a outline of how to reproduce my solution for the <Competition Name> competition.
If you run into any trouble with the setup/code or have any questions please contact me at <email>

#ARCHIVE CONTENTS
kaggle_model.tgz          : original kaggle model upload - contains original code, additional training examples, corrected labels, etc
comp_etc                     : contains ancillary information for prediction - clustering of training/test examples
comp_mdl                     : model binaries used in generating solution
comp_preds                   : model predictions
train_code                  : code to rebuild models from scratch
predict_code                : code to generate predictions from model binaries

#HARDWARE: (The following specs were used to create the original solution)
Ubuntu 16.04 LTS (512 GB boot disk)
n1-standard-16 (16 vCPUs, 60 GB memory)
1 x NVIDIA Tesla P100

#SOFTWARE (python packages are detailed separately in `requirements.txt`):
Python 3.5.1
CUDA 8.0
cuddn 7.1.4.18
nvidia drivers v.384
-- Equivalent Dockerfile for the GPU installs: https://gitlab.com/nvidia/cuda/blob/ubuntu16.04/8.0/runtime/cudnn7/Dockerfile

#DATA SETUP (assumes the [Kaggle API](https://github.com/Kaggle/kaggle-api) is installed)

below are the shell commands used in each step, as run from the top level directory

```
mkdir -p data/stage1/
cd data/stage1/
kaggle competitions download -c <competition name> -f train.csv
kaggle competitions download -c <competition name> -f test_stage_1.csv
```

```
mkdir -p data/stage2/
cd ../data/stage1/
kaggle competitions download -c <competition name> -f test_stage_2.csv
cd ..
```

#DATA PROCESSING
# The train/predict code will also call this script if it has not already been run on the relevant data.
python ./train_code/prepare_data.py --data_dir=data/stage1/ --output_dir=data/stage1_cleaned

#MODEL BUILD: There are three options to produce the solution.
1) very fast prediction
    a) runs in a few minutes
    b) uses precomputed neural network predictions
2) ordinary prediction
    a) expect this to run for 1-2 days
    b) uses binary model files
3) retrain models
    a) expect this to run about a week
    b) trains all models from scratch
    c) follow this with (2) to produce entire solution from scratch

shell command to run each build is below
#1) very fast prediction (overwrites comp_preds/sub1.csv and comp_preds/sub2.csv)
python ./predict_code/calibrate_model.py

#2) ordinary prediction (overwrites predictions in comp_preds directory)
sh ./predict_code/predict_models.sh

#3) retrain models (overwrites models in comp_model directory)
sh ./train_code/train_models.sh
