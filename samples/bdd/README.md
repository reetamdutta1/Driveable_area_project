# drivable-area-segmentation
This project implements driving area segmentation by applying on transfer learning on [Mask R-CNN](https://arxiv.org/abs/1703.06870) model [here](https://github.com/matterport/Mask_RCNN). This work was done as a part of [CS230 class at Stanford](https://cs230.stanford.edu/), winter 2019. Project report can be found [here](https://github.com/beeRitu/drivable-area-segmentation/blob/master/CS230_Project_Report_Winter_2018.pdf) on GitHub and is also posted on [class past projects page](https://cs230.stanford.edu/past-projects/#winter-2019) as of on the day of writing this update. We have used [Berkeley Driving Dataset](http://bdd-data.berkeley.edu/), for rest of the project acronym BDD has been used. 
![Instance Segmentation Sample](assets/DrivableArea.JPG)

The repository includes:
* [inspect_bdd_data.ipynb](inspect_bdd_data.ipynb) visualize the training data and masks
* [inspect_bdd_model.ipynb](inspect_bdd_model.ipynb) visualizes the detection pipeline at every step
* [inspect_bdd_results.ipynb](inspect_bdd_results.ipynb) analyzes segmentation results on the validation dataset.
* [shrink.ipynb](shrink.ipynb) creates label files from the master label file corresponding to images in a folder. 
* [bdd.py](bdd.py) Training code for BDD 
# Getting started 
In order to run transfer learning on [Mask R-CNN](https://arxiv.org/abs/1703.06870) 
## Clone matterport Mask-RCNN repo
```bash
git clone https://github.com/matterport/Mask_RCNN
```
## Clone this repository 
```bash
cd /Mask_RCNN/samples/
mkdir bdd
git clone https://github.com/beeRitu/drivable-area-segmentation
```
Ensure all the jupyter notebooks are in the folder 
```bash
/Mask_RCNN/samples/bdd
```
## Download the dataset
Dataset can be downloaded from [Berkeley Driving Dataset](http://bdd-data.berkeley.edu/). There are 70k training images, 10k validation images and 20k test images. These should be copied in the following path 
```bash
/Mask_RCNN/datasets/bdd/train #70,000 training images
/Mask_RCNN/datasets/bdd/val #10,000 training images
/Mask_RCNN/datasets/bdd/test #20,000 training images
```
In addition, copy the 2 label files in the path indicated below
```bash
/Mask_RCNN/datasets/bdd/bdd100k_labels_images_train.json #labels for all 70,000 training images
/Mask_RCNN/datasets/bdd/bdd100k_labels_images_val.json #labels for all 10,000 validation images
```
## Visualize the training data 
You can create a small subset of the training data for the initial visualization. To do this, rename the folder `train` containging 70k training images to something like `train70k` and copy a subset of training images to the folder `train`. 1000 images might be a good starting point. Run shrink.ipynb to create a shortened version of the label file corresponding to the number of images in `train`. This will create file named `via_region_data.json` in folder `train`. Check the following assignments before running the notebook
```bash
master_json="bdd100k_labels_images_train.json" # label file for 70k images in /Mask_RCNN/datasets/bdd/
dataset_dir=os.path.abspath("../../datasets/bdd/train") # image folder where json file will be created
output_json="via_region_data" # Name of the output json file 
```
## Training the model 
We started the training from pretrained MS COCO models. Training and evaluation code is in `samples/bdd/bdd.py`. Training can be started directly from command line as such:
```
# Train a new model on bdd dataset starting from pre-trained COCO weights
python3 samples/bdd/bdd.py train --dataset=/datasets/bdd/ --weights='coco'

# Continue training a model that you had trained earlier
python3 samples/bdd/bdd.py train --dataset=/datasets/bdd/ --weights=logs/<weights directory>/<weights.h5 file to be used>

```
The training schedule, learning rate, and other parameters should be set in `samples/bdd/bdd.py`.
## Visualizing the model
Model can be visualized by running the notebook [inspect_bdd_model.ipynb](inspect_bdd_model.ipynb). It generates results for the intermediate and final steps in the model.
## Results and Analysis 
Results can be analyzed by running the notebook [inspect_bdd_results.ipynb](inspect_bdd_results.ipynb)
