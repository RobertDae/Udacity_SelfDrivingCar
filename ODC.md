# **Self-Driving Car**
# **Capstone Project: Programming A Self Driving Car**

## RD

Others who would like to either replicate or use a modified version of the below procedure, please read through the complete write-up before attempting to build this object detection classifier

## Traffic Light Detection and Classification

Overview

Discussion and details about the Object Detection and Classification performed as part of the Capstone Project: Programming A Self Driving Car.

As the car traverses along the test track inside the simulator, it has to detect traffic-lights and classify the light state of the detected traffic-light. This is performed using images from the front camera. Depending on the light state, the car stops at the traffic-light stop-line if it is "Red" or moves forward if it is either "Green or Yellow".

[//]: # (Image References)

[image1]: ./Writeup_Img/SpeedAccuracy_COD.png "SpeedAccuracy_COD"
[image2]: ./Writeup_Img/TFV_1p13p2.png "TFV_1p13p2"
[image3]: ./Writeup_Img/MyOD6_GCL_TB1.png "MyOD6_GCL_TB1"
[image4]: ./Writeup_Img/MyOD6_GCL_TB2.png "MyOD6_GCL_TB2"
[image5]: ./Writeup_Img/MyOD6_GCL_TB3.png "MyOD6_GCL_TB3"
[image6]: ./Writeup_Img/TFV_1p4.png "TFV_1p4"
[image7]: ./Writeup_Img/MyOD7_IG.png "MyOD7_IG"
[image8]: ./Writeup_Img/MyOD6_M1.png "MyOD6_M1"

#
Tensorflow Object Detection API and Tensorflow detection model zoo

The traffic light detection and classification was performed using Tensorflow Object Detection API in conjunction with Tensorflow detection model zoo. 

[Tensorflow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection)

Creating accurate machine learning models capable of localizing and identifying multiple objects in a single image remains a core challenge in computer vision. The TensorFlow Object Detection API is an open source framework built on top of TensorFlow that makes it easy to construct, train, and deploy object detection models

[Tensorflow detection model zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md)

A collection of detection models pre-trained on the [COCO
dataset](http://mscoco.org), the [Kitti dataset](http://www.cvlibs.net/datasets/kitti/),
the [Open Images dataset](https://github.com/openimages/dataset), the
[AVA v2.1 dataset](https://research.google.com/ava/) and the
[iNaturalist Species Detection Dataset](https://github.com/visipedia/inat_comp/blob/master/2017/README.md#bounding-boxes).
These models can be useful for out-of-the-box inference for categories already in those datasets  and also for initializing models when training on novel datasets.

#
Selection of model from Tensorflow detection model zoo

The criteria for selection of model from the Tensorflow detection models zoo was based on the work by Jonathan Huang et al.,
[Speed/accuracy trade-offs for modern convolutional object detectors](https://arxiv.org/pdf/1611.10012.pdf).
 
 A summary of results from the above work is shown below:
![][image1]

Based on the above set of results the "Faster R-CNN w/ResNet, Hi Res, 50 Proposals" model was selected for traffic light detection and classification. This model is a good trade-off between speed and accuracy. Accordingly, the [faster_rcnn_resnet50_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_coco_2018_01_28.tar.gz) was utilized as initializer for training the dataset.

#
Train and Test Data Sets

Training Data Set

The training data set was obtained from the following source [Link](https://github.com/coldKnight/TrafficLight_Detection-TensorFlowAPI). A few modifications were performed, such that the number of images consisting of Green and Yello traffic lights are equal. In addition the images in the data set were not labled and classified. The image labeling was performed using [LabelImg](https://github.com/tzutalin/labelImg). The final data set used for training can be accessed at [Link](https://drive.google.com/open?id=1kDV6NReRehsExP-mdMYqoYGszpVZhaEp)

Test Data Set

The test data set was obtained from the following source [Link](https://github.com/alex-lechner/Traffic-Light-Classification). The final data set used for testing can be accessed at [Link](https://drive.google.com/open?id=1cg_Owcn-nXXpufHn2g6D7ElVE65ZWqag)

Ideal Data Set

Ideally, simulator data set should have been collected by running the car in autonomous mode and taking advantage of traffic light state and looping the car around the test track. ROS code has been already implemented to loop the car around the test track. Capture traffic light images only when the waypoint difference between the car current location and the traffic-light stop line is between 100 and 0. Save images with file name consisting of traffic-light id, waypoint difference, traffic-ligth state. ROS code has already been implemented to perform saving images with file name as described previously. Data collected in such a manner might be dominated by "Red" and "Green" light states. To overcome this let the car traverse couple of laps and save images only when light state is "Yellow". To further minimze the bias between "Red", "Green", and "Yellow" a wrapper-code can be written such that it picks images from the collected data set as follows: for each traffic light id create buckets such that each bucket represents waypoint difference in increments of 5. Therefore for 100 waypoint difference, there should be about 20 buckets. For each bucket, from the saved images data set,select only 3 images representing "Red", "Green", and "Yellow" light states. This will result in about a total of 480 images (8 traffic lights, 20 waypoint difference buckets, 3 light states). The previously described procedure should result in consistent test data without any bias.

However, by the time all necessary code has been implemented, the remainder of the project has been completed and had to forgo the above described procedure.

#
Training

All the training was performed on [Google Colab](https://colab.research.google.com/notebooks/welcome.ipynb) using a GPU (GPU is usually a NVIDIA Tesla P100). However, all the preliminary or preprocessing work was performed locally on windows machine.

Some of the preliminary and preprocessing work was performed based on this [tutorial](https://medium.com/object-detection-using-tensorflow-and-coco-pre/object-detection-using-tensorflow-and-coco-pre-trained-models-5d8386019a8) at medium.com

Important Notes:
- Installed Anconda for windows [Link](https://www.anaconda.com/distribution/). Did not use Anconda GUI for this project, but rather the most useful tool from Anconda installation was the use of "Conda Environments". Using "Conda Environments" it was easy to maintain different Tensor Flow versions and its associated dependencies.
- The TensorFlow models repository's code (which contains the object detection API) is continuously updated by the developers. Sometimes they make changes that break functionality with old versions of TensorFlow [Link](https://github.com/EdjeElectronics/TensorFlow-Object-Detection-API-Tutorial-Train-Multiple-Objects-Windows-10).
- In addtion, the older TensorFlow models repositories do not have all the latest object detection models (pre-trained classifiers with specific neural network architectures).
- In this work, instead of using the latest Tensor Flow models, training was performed using Tensor Flow models based on [version 1.13](https://github.com/tensorflow/models/tree/r1.13.0), one version below the latest version as of 12/07/2019. For a complete guide on Tensor Flow models based on various versions of Tensor Flow refer to this [work](https://github.com/EdjeElectronics/TensorFlow-Object-Detection-API-Tutorial-Train-Multiple-Objects-Windows-10)

Training: Tools and Procedure
- Tensor Flow version 1.13.2 and the necessary packages to run the object detection API are shown below
![][image2]
- Create conda environment with the above listed packages and execute the below set of commands sequentially using Anaconda command prompt:
  - `set PYTHONPATH=C:/Users/???/Desktop/CarND/MyOD6_GCL/models`
  - `set PYTHONPATH=C:/Users/???/Desktop/CarND/MyOD6_GCL/models/research`
  - `set PYTHONPATH=C:/Users/???/Desktop/CarND/MyOD6_GCL/models/research/slim`
  -  `set PATH=%PATH%;PYTHONPATH`
  - `echo %PATH%`
  - `echo %PYTHONPATH%`
  - `cd C:/Users/???/Desktop/CarND/MyOD6_GCL/models/research`
  - `for /f %i in ('dir /b object_detection\protos\*.proto') do protoc object_detection\protos\%i --python_out=.`
  - `python setup.py build`
  - `python setup.py install`
  - `cd object_detection`
  - `python xml_to_csv.py`
  - `python generate_tfrecord.py --csv_input=images/train_labels.csv --image_dir=images/train --output_path=train.record`
  - `python generate_tfrecord.py --csv_input=images/test_labels.csv --image_dir=images/test --output_path=test.record`
  - `python legacy/train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/faster_rcnn_resnet50_coco.config`
  
- To keep track of the above training process and various loss variables, open another Anaconda command prompt and execute below set of commands
  - `cd C:/Users/???/Desktop/CarND/MyOD6_GCL/models/research/object_detection/`
  - `tensorboard --logdir=training`
- Executing the above two commands would result in a http address with port 6006. The training progress and other loss variables can be viewed using a browser and the http address.

Frozen Inference Graph
- After the training is completed a frozen inference graph can be created using the below command:
  - `python export_inference_graph.py --input_type image_tensor --pipeline_config_path training/faster_rcnn_resnet50_coco.config --trained_checkpoint_prefix training/model.ckpt-???? --output_directory inference_graph`
  - `model.ckpt-????` represents one of the saved model checkpoints from the training process. Typically this model-checkpoint refers to one with the lowest total loss.

Testing Classifier
- After the inference graph has been created, test the classifier on images using the below command
- `python my_Object_detection_image.py`
- The above python code typically is written to test on a single image. If it is necessary to test the classifier on multiple images in a single session use the following python code [my_Object_detection_image.py](https://drive.google.com/open?id=1D4pAmQ_Cauqpcxp0hO5u5u3_YuXcupnk)
- Results from the classifier output can be accessed at this [Link](./ODC_Test.md)

Google Colab
- All the training has been performed in Google Colab (Colab) because of the ease of access to GPU
- After the training has been tested on local machine, upload the working folder as a zip file to Google Drive. It is easier to upload to Google Drive and doing a transfer from Google Drive to Colab rather than uploading directly to Colab
- Training in Colab is performed in iPython notebook. iPython notebook used for this work can be accessed [here](./Writeup_IV/od6_req.ipynb). Execute the cells one at a time 

Training results from Google Colab: it shows Total Loss as a function of number of steps/iterations. The model-checkpoint for generating the inference graph was selected approximately at step number 70,000. The selected model-checkpoint was 69,544. The total loss at step number 69,544 was approximately 0.02

![][image3]

<!-- 
![][image4])
![][image5]
-->

By running the evaluation on the test and train data sets, the following mertrics were obtained:
- Train Data Set

`Average Precision  (AP) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.981`

 `Average Precision  (AP) @[ IoU=0.50      | area=   all | maxDets=100 ] = 1.000`
 
 `Average Precision  (AP) @[ IoU=0.75      | area=   all | maxDets=100 ] = 0.997`
 
 `Average Precision  (AP) @[ IoU=0.50:0.95 | area= small | maxDets=100 ] = 0.942`
 
 `Average Precision  (AP) @[ IoU=0.50:0.95 | area=medium | maxDets=100 ] = 0.997`
 
 `Average Precision  (AP) @[ IoU=0.50:0.95 | area= large | maxDets=100 ] = 1.000`
 
 `Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=  1 ] = 0.340`
 
 `Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets= 10 ] = 0.984`
 
 `Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.984`
 
 `Average Recall     (AR) @[ IoU=0.50:0.95 | area= small | maxDets=100 ] = 0.953`
 
 `Average Recall     (AR) @[ IoU=0.50:0.95 | area=medium | maxDets=100 ] = 0.998`
 
 `Average Recall     (AR) @[ IoU=0.50:0.95 | area= large | maxDets=100 ] = 1.000`
 
`INFO:tensorflow:DetectionBoxes_Precision/mAP: 0.981119`

`INFO:tensorflow:DetectionBoxes_Precision/mAP (large): 1.000000`

`INFO:tensorflow:DetectionBoxes_Precision/mAP (medium): 0.996519`

`INFO:tensorflow:DetectionBoxes_Precision/mAP (small): 0.941960`

`INFO:tensorflow:DetectionBoxes_Precision/mAP@.50IOU: 0.999881`

`INFO:tensorflow:DetectionBoxes_Precision/mAP@.75IOU: 0.996602`

`INFO:tensorflow:DetectionBoxes_Recall/AR@1: 0.339729`

`INFO:tensorflow:DetectionBoxes_Recall/AR@10: 0.984217`

`INFO:tensorflow:DetectionBoxes_Recall/AR@100: 0.984217`

`INFO:tensorflow:DetectionBoxes_Recall/AR@100 (large): 1.000000`

`INFO:tensorflow:DetectionBoxes_Recall/AR@100 (medium): 0.997663`

`INFO:tensorflow:DetectionBoxes_Recall/AR@100 (small): 0.953016`

- Test Data Set
  - PascalBoxes_PerformanceByCategory/AP@0.5IOU/b'Green': 0.887854
  - PascalBoxes_PerformanceByCategory/AP@0.5IOU/b'Red': 0.945645
  - PascalBoxes_PerformanceByCategory/AP@0.5IOU/b'Yellow': 0.938653
  - PascalBoxes_Precision/mAP@0.5IOU: 0.924051
  
Complete objection detection work, train and test data sets, and saved model checkpoints during the training process can be accessed at [Link:6GB](https://drive.google.com/open?id=1bCtRdwh8hNPNMM1vyB2XpPK6EGPi2RQg) and [Link:2GB](https://drive.google.com/open?id=19qdeWJjwXhFJsSOaCy5niypHlKFB7hRx)

#
Saving Frozen Inference Graph for TensorFlow Version 1.3.0
- Udacity supplied Ubuntu image with ROS installation uses TensorFlow version 1.3.0
- The lowest TensorFLow version with Object Detection API is version 1.4.0. This can be accessed at [Link](https://github.com/tensorflow/models/tree/1f34fcafc1454e0d31ab4a6cc022102a54ac0f5b)
- It has also been verified that the frozen inference graph generated using TensorFlow version 1.4.0 also works with version 1.3.0
- Complete the object detection training as described in the previous section using TensorFlow version 1.13.2
- Make a copy of the working directory where the training has been completed
- Tensor Flow version 1.4.0 and the necessary packages to run the object detection API are shown below

![][image6]

- Create conda environment with the above listed packages and execute the below set of commands sequentially using Anaconda command prompt:
  - `set PYTHONPATH=C:/Users/???/Desktop/CarND/MyOD7_GCL/models`
  - `set PYTHONPATH=C:/Users/???/Desktop/CarND/MyOD7_GCL/models/research`
  - `set PYTHONPATH=C:/Users/???/Desktop/CarND/MyOD7_GCL/models/research/slim`
  -  `set PATH=%PATH%;PYTHONPATH`
  - `echo %PATH%`
  - `echo %PYTHONPATH%`
  - `cd C:/Users/???/Desktop/CarND/MyOD7_GCL/models/research`
  - `for /f %i in ('dir /b object_detection\protos\*.proto') do protoc object_detection\protos\%i --python_out=.`
  - `python setup.py build`
  - `python setup.py install`
  - `cd object_detection`
- Frozen Inference Graph. This is the most important step. Once the training has been completed using TensorFlow version 1.13.2, DO NOT rerun the training command in version 1.4.0. Instead run the below command
  - `python export_inference_graph.py --input_type image_tensor --pipeline_config_path training/faster_rcnn_resnet50_coco.config --trained_checkpoint_prefix training/model.ckpt-69544 --output_directory inference_graph`

After creation of the inference graph, the 'inference_graph' folder should look something similar to as shown below:

![][image7]

Complete objection detection work, train and test data sets, saved model checkpoints, and frozen inference graph can be accessed at [Link:6GB](https://drive.google.com/open?id=1btVy0C9Y32uWW3BC3G7heQCCx_LzJX6o) and [Link:2GB](https://drive.google.com/open?id=1ck5yf1BYGU1JRX0i82dAJm6fOo86ejzY)

# 
Must Read

- The above object detection classifier used [faster_rcnn_resnet50_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_coco_2018_01_28.tar.gz) as the initializer for training the traffic lights data set. There might be scenarios and use cases where it would be necessary to depend on other [trained models](https://github.com/tensorflow/models/blob/v1.13.0/research/object_detection/g3doc/detection_model_zoo.md)
- To integrate other models into the above described framework, download the intended pre-trained model from [Tensorflow detection model zoo (TensorFlow version 1.13.0)](https://github.com/tensorflow/models/blob/v1.13.0/research/object_detection/g3doc/detection_model_zoo.md) and untar/unzip the pre-trained model files into the folder `MyOD6_M1` and it contents should look as shown below

![][image8]

- In addition, copy the file `pipline.config` from `\MyOD6_GCL\models\research\object_detection\MyOD6_M1` to folder `\MyOD6_GCL\models\research\object_detection\training` and rename it as `faster_rcnn_resnet50_coco.config`
- Changes are performed to the configuration file `faster_rcnn_resnet50_coco.config`. Some of these changes are: number of classes (for traffic light classification: 3), image size (for traffic light classification 800X600), set batch_size=1 for object detection, number of steps, etc. The `faster_rcnn_resnet50_coco.config` can be accessed at [Link](https://drive.google.com/open?id=1rt_sio7nozcRc6ToSlncPOhVJOwqYkHq)
- Reference to the folder `MyOD6_M1` is made inside the file `faster_rcnn_resnet50_coco.config` as follows `fine_tune_checkpoint: "/content/MyOD6_GCL/models/research/object_detection/MyOD6_M1/model.ckpt"`
- Every new instance of training or generating an inference graph, the folders `\MyOD6_GCL\models\research\object_detection\training` and `\MyOD6_GCL\models\research\object_detection\inference_graph` needs to be emptied. Only exception being files `faster_rcnn_resnet50_coco.config` and `labelmap.pbtxt` in the folder `\MyOD6_GCL\models\research\object_detection\training` should not be deleted
- There are other folders and files that should be cleaned prior to initiating the training process. The previous couple of steps and most of the above listed classification procedure has been clearly described in this [tutorial](https://medium.com/object-detection-using-tensorflow-and-coco-pre/object-detection-using-tensorflow-and-coco-pre-trained-models-5d8386019a8)
- It's important that the directory structure starting from `\models\` and its subdirectories must be maintained. Naming convention for the folders `\MyOD6_GCL\` and `\MyOD6_M1\` is just a coincidence and can be renamed
