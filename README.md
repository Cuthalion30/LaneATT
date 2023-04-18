# <div align="center"> LaneATT Using R.C. Car Data </div>

This repository started as a fork of https://github.com/lucastabelini/LaneATT which contains the source code for LaneATT, a state-of-the-art lane detection model.  We have updated the source code to include the ability to test a model on our novel dataset.  This data is located in the "datasets" folder in this repository.  Our work is described in the research paper "_From RC to AV: Utilizing free space and lane detection machine learning models on data retrieved by a remote-controlled car_" by [Nathanael Kovscek](https://github.com/Cuthalion30), [Adam Kenawell](https://github.com/Ankenawell).

### Table of Contents
1. [Prerequisites](#1-pre-requisites)
2. [Setup](#2-setup)
3. [Testing](#3-testing)
4. [Results](#4-results)
5. [Citation](#5-citation)

### 1. Pre-requisites
- A penn state account
- Access to Penn State's ROAR supercomputer.  Follow the steps on this link to get access: https://www.icds.psu.edu/computing-services/roar-user-guide/getting-an-account-penn-state-faculty-staff-and-students/
- Other dependencies as outlined in `requirements.txt`

### 2. Setup
1. Log into your ICDS Penn State ROAR account. Do this by following the below link and starting an RHEL7 Jupyter Server. Keep all field in the submit job form the same, EXCEPT FOR the node type. This must be changed to “ACI-b GPU Core (NO GL Acceleration)”. Once you are logged in, start a terminal here: https://as1.fim.psu.edu/idp/profile/SAML2/Redirect/SSO?execution=e1s1&_eventId_proceed=1

2. Change directory to the location where we have shared the reproducible code by typing this
command into terminal: 
```bash
cd /gpfs/scratch/nak5437/LaneATT
```

3. Next, you must set up a conda environment to ensure you are using the correct versions of
packages and python. Do this with the following commands:

```bash 
conda create -n laneatt python=3.8 -y 
conda activate laneatt 
conda install pytorch==1.6 torchvision -c pytorch
```

4. Now we must make sure we are using the correct compiler for the c++ code. Do this by running
this command:

```bash 
module load gcc/8.3.1
```

5. There are a few more packages and setup we must do before we can train and test a model.
Do this by running these commands from the base folder that we already cd’ed into
(gpfs/scratch/nak5437/LaneATT):

```bash 
pip install -r requirements.txt
cd lib/nms
python setup.py install
cd ..
cd ..
```


### 3. Testing 
1. Now all of the setup should be complete, and by running the below line of code, you will test a
pretrained model that was used in the research paper. By testing this model, you will observe
the exact same accuracy as the results in the paper:
```bash
python main.py test --exp_name laneatt_r34_tusimple
```
2. If you run into an error stating "downgrade the protobuf package to 3.20.x or lower" then run this command:
```bash
pip install protobuf==3.19.0
```
And then rerun step 1 of this section.


### 4. Results

Listed below is the results table on the TuSimple dataset.  This is able to perform so well due to the velocity of data, as well as the test data is a subset of the train data:
|   Backbone    |      Accuracy (%)     |      FDR (%)     |      FNR (%)     |      F1 (%)     | FPS |
|    :---       |         ---:          |       ---:       |       ---:       |      ---:       | ---:|
| ResNet-18     |    95.57              |    3.56          |    3.01          |    96.71        | 250 |
| ResNet-34     |    95.63              |    3.53          |    2.92          |    96.77        | 171 |
| ResNet-122    |    96.10              |    4.64          |    2.17          |    96.06        | 26 |

Below is a table of results on our own data. There are numerous possibilities for its low performance.  It could be due to bad ground truth labeling, bad lane quality and image quality, and also just the fact that this data is very different from the TuSimple data it was trained on:
|   Backbone    |      Accuracy (%)     |      FDR (%)     |      FNR (%)     |  FPS |
|    :---       |         ---:          |       ---:       |       ---:       |  ---:|
| ResNet-34     |    12.27              |    3.53          |    2.92          | 1000 |


### 5. Citation
If you use this code in your research, please cite:

```bibtex
@InProceedings{
  author    = {Nathanael Kovscek
               and Adam Kenawell},
  title     = {{From RC to AV: Utilizing free space and lane detection machine learning models on data retrieved by a remote-controlled car}},
  year      = {2023}
}