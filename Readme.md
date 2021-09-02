Hi Everyone!<br>
This is a repository for my bachelor degree's final project (called Skripsi in Indonesia). I am trying to detect *tuberculosis bacteria* from *sputum images* using Faster R-CNN algorithm. I used TensorFlow 2 Object Detection API written in Python and developed fully using Google Colaboratory.
## The Dataset
I used a private dataset from colleague. The images dimension are 416 x 480 px with COCO annotations (saved in xml files for each images).  Then I used [RoboFlow](https://app.roboflow.com/) to do image augmentations. Then I exported the training and validation dataset into **TFRecord Format**, and the testing dataset into TensorFlow Object Detection format (all annotations into a single csv file).

## Backbone
I used Resnet50 for the backbone here. You can choose the models and backbone from [TensorFlow Object Detection Model Zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md).

## Other Configuration
The configurations are explained directly in the notebook, but for short these are the things I configured a bit:
-   pipeline_file: defined above in writing custom training configuration
-   model_dir: the location tensorboard logs and saved model checkpoints will save to
-   num_train_steps: how long to train for
-   num_eval_steps: perform eval on validation set after this many steps 

## Inference
For the inference, I exported the detection result (coordinates and label) into a single csv file, then plot it to the images. 
