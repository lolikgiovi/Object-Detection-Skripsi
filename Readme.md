This is a repository for my bachelor degree's final project (called Skripsi in Indonesia). I am trying to detect *tuberculosis bacteria* from *sputum images* using Faster R-CNN algorithm. I used TensorFlow 2 Object Detection API written in Python and developed fully using Google Colaboratory.

## Background
Based on [WHO Global Tuberculosis Report](https://www.who.int/publications-detail-redirect/9789240013131), Tuberculosis has always been a communicable disease with a high prevalence in Indonesia. Indonesia got the 2nd rank on Tuberculosis Prevalence in the world (8.5% of whole tuberculosis case occured in Indonesia). 

There are three main steps in handling Tuberculosis:
1. Screening: Self-asses whether a person had tuberculosis symptoms
2. Testing: To confirm if the person really has tuberculosis
3. Treatment: Medication

With the pandemic that hit us in the last two years, tuberculosis testing rate has fallen a lot (moreover in Indonesia, compared to other country).

## About Tuberculosis Testing
There are several methods to run tuberculosis testing, such as using *chest X-Ray*, or doing some *Mollecular Test* (yea I know that sounds fancy), those tests can only be done in the center of health facility thou (like some big hospital) with a high cost.

There are also a cheaper way testing method such as *tuberculin skin test* and *sputum smear bacteria counting*. The *tuberculin skin test* is cheap and the result can be reported within 36 hours, but then the false positive and negative rate are high and this method is less acurate.

### *Sputum Smear Bacteria Counting*
Meanwhile the *sputum smear bacteria counting* is pretty much realiable method, but the process is labor-intensive (a technician should count the amount of bacteria in three slides of specimens in the microscope).
This is the example of what technician usually sees in one *field of view*:

![Image of sputum smear specimen](https://github.com/lolikgiovi/Object-Detection-Skripsi/blob/main/Resources/00018.jpg?raw=true)
The bacteria colored red, meanwhile anything other than bacteria (called background) *ideally should* be light-blue colored. But for most cases, there are too much staining that make the background become too blue. The bacteria itself ideally should be colored bright red, but it could be too light to be seen.

With that kind of situation, the technician could have produced a wrong judgment to count bacteria that led to a number of false negatives. Since they have to count around **300 *field of views***, it could be an exhausting work and led to a human error problem.
> **Problem Statement** for this project:
>  A high burden of work and un-ideal specimens condition could lead to human-error false diagnosis of tuberculosis based on sputum images analysis. Hence, an assistive device could help the technician to perform a sputum image analysis.

> **Problem Scope**: 
> Since there are medical ethics to automate stuffs, this project intend to develop an assistive device to help technician perform sputum image analysis by implementing deep learning method to *automatically detect tuberculosis bacteria from sputum images* and assist the technician to do their job.

## Method

### The Dataset
I used a private dataset from [a colleague](https://aip.scitation.org/doi/10.1063/5.0036388). The images dimension are 416 x 480 px with [Pascal VOC annotations format](https://towardsai.net/p/machine-learning/understanding-pascal-voc-and-coco-annotations-for-object-detection) (saved in xml files for each images).  Then I used [RoboFlow](https://app.roboflow.com/) to do image augmentations. Then I exported the training and validation dataset into **TFRecord Format**, and the testing dataset into TensorFlow Object Detection format (all annotations into a single csv file).

Dataset Preview:

![Dataset Preview](https://github.com/lolikgiovi/Object-Detection-Skripsi/blob/main/Resources/Dataset%20from%20Roboflow.jpg?raw=true)

### TensorFlow2 Object Detection API
I am using TensorFlow2 Object Detection API as the main structure of the project. The documentations can be found [here](https://tensorflow-object-detection-api-tutorial.readthedocs.io/en/latest/)

### Backbone
I used Resnet50 for the backbone here. You can choose the models and backbone from [TensorFlow Object Detection Model Zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md).

### Other Configuration
The configurations are explained directly in the notebook, but for short these are the things I configured a bit:
-   pipeline_file: defined above in writing custom training configuration
-   model_dir: the location tensorboard logs and saved model checkpoints will save to
-   num_train_steps: how long to train for
-   num_eval_steps: perform eval on validation set after this many steps 

## Output
After training the model, I manually plot the detection result on the images and count the confusion matrix score of it. The result is:
| Category | Count|
|--|--|
| True Positive | 66 |
| False Positive | 4 |
| True Negative | 0 |
| False Negative | 5 |

Then, I calculated the recall (sensitivity), precision (specificity for one class) and F-Score. Found the values of:
| Metric| Score (Percentage)|
|--|--|
| recall  | 93 |
| precision  | 94 |
| F-Score | 94 |

From detecting single class (tuberculosis bacteria) in sputum images, the model reached those score metrics.
- Recall score is interpreted as how sensitive a model to detect any object (if there are more than one classes) in one image, since recall is comparing true detections with all the detections generated.
- Precision is interpreted as how precise (how true) a model to detect an object and classify it from others (if there are more than one object), cause precision is comparing true detections with all the actual detection should be.
- And F-Score is a *weighted score* for recall and precision. 

### Object Detection Inference
For the inference, I exported the detection result (coordinates and label) into a single csv file, then plot it to the images. 

## Social Impact
For the impact of this project, I imagine that it could be implemented in the basic health service in Indonesia such as Puskesmas so there could be higher testing rate of tuberculosis in Indonesia.
