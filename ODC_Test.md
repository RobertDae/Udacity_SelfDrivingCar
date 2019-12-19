# **Self-Driving Car**
# **Capstone Project: Programming A Self Driving Car**

## RD

## Traffic Light Detection and Classification: Classifier Testing Results

Results summary of Traffic Light Detection and Classification classifier performance on test images:
- In most cases, output of the classifier matches that of the ground truth values
- Misclassifications are in instances where the distance between the car and the traffic light is quite significant. Even, in those instances "Red" and "Yellow" light states are being classified correctly. Missclassifications occurs only with the "Green" light state being classified as "Yellow". This should not be a concern, as classification is not performed untill the waypoint difference bteween the car and traffic light is less than 75. At 75 waypoints difference, car is pretty close to the traffic light
- All the classifier results on test images can be accessed at this [Link](./CImages_Test/)

[//]: # (Image References)

[image1]: ./CImages_Test/cimgs_11.jpg "cimgs_11"
[image2]: ./CImages_Test/cimgs_61.jpg "cimgs_61"
[image3]: ./CImages_Test/cimgs_63.jpg "cimgs_63"
[image4]: ./CImages_Test/cimgs_19.jpg "cimgs_19"
[image5]: ./CImages_Test/cimgs_20.jpg "cimgs_20"
[image6]: ./CImages_Test/cimgs_21.jpg "cimgs_21"
[image7]: ./CImages_Test/cimgs_26.jpg "cimgs_26"
[image8]: ./CImages_Test/cimgs_29.jpg "cimgs_29"
[image9]: ./CImages_Test/cimgs_27.jpg "cimgs_27"
[image10]: ./CImages_Test/cimgs_902.jpg "cimgs_902"

[image11]: ./CImages_Test/cimgs_18.jpg "cimgs_18"
[image12]: ./CImages_Test/cimgs_31.jpg "cimgs_31"
[image13]: ./CImages_Test/cimgs_56.jpg "cimgs_56"
[image14]: ./CImages_Test/cimgs_116.jpg "cimgs_116"
[image15]: ./CImages_Test/cimgs_23.jpg "cimgs_23"

#
Classifications

![][image1]
![][image2]
![][image3]
![][image4]
![][image5]
![][image6]
![][image7]
![][image8]
![][image9]
![][image10]

#
Misclassifications

![][image11]
![][image12]
![][image13]
![][image14]
![][image15]
