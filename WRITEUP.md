# Project Write-Up

This project can find the number of people and the average time for which they are in the frame. This can be deployed in any area where we can read from camera.

## Explaining Custom Layers

There are few supported layers which are supported by openVino but few are not supported by it. There are few other layers which are not supported by hardware. We can handle unsupported layer in two ways:
1. using custom layer
2. running the layer in its original framework
3. Hardware extension for few layers

The process behind converting custom layers involves...
   So, model optimizer filter out all the layers which are not in supported layer. And those are treated as custom layer.
   Adding a custom layer is dependent on the orginal framework:
    1. Register an extension to model optimizer-- common for both
    2. for Caffe: Register the layer as custom and then use caffe to calculate the output shape
    3. Tensorflow: Replace unsupported subgraph with another subgraph

Some of the potential reasons for handling custom layers are...
  it is possible that IR doesn't support all the layers from original framework. Sometimes because of hardware, for example on cpu there are few IR model which are directly supported but others may not. 

## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations
were...

The difference between model accuracy pre- and post-conversion was...

The size of the model pre- and post-conversion was...
1. Model: ssd_mobilenet_v2_coco_2018_03_29
    size-before: 180 Mb (zip file)
    size-after: 65 Mb

2. Model: TensorFlow Resnet v2
    size-before: 350 Mb
    size-after: 180Mb

3. Person detection retail -- Intel
    I din't compare the size for this model as I downloaded the converted model itself.

The inference time of the model pre- and post-conversion was...
1. Model: ssd_mobilenet_v2_coco_2018_03_29
   avg_infer time: 69-70 ms
          
2. Model: TensorFlow Resnet v2
   avg_infer time: 45-47 ms
3. Person detection retail -- Intel
   avg_infer time: 2600 ms


## Assess Model Use Cases

In an essence this app would be useful in all the places where number of people keep changing and having count of them is useful. There are few use case of this application:
1. In retails shops
2. Hospital area: if number of people are increasing in a restricted area then it can be used to trigger an alarm based on the people count.
3. It can be used in public places, specially in current time as we need to maintain the social distancing (because of Covid-19), it can be useful to restric number of people in an area.


## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a deployed edge model. The potential effects of each of these are as follows...

This is a pedestrian detector for the Retail scenario. It is based on MobileNetV2-like backbone that includes depth-wise convolutions to reduce the amount of computation for the 3x3 convolution block. The single SSD head from 1/16 scale feature map has 12 clustered prior boxes.

**Pose coverage**: Standing upright, parallel to image plane
**Support of occluded pedestrians**: yes

## Model Research

In investigating potential people counter models, I tried each of the following three models:

- Model 1: Tensorflow SSD Mobilenet V2
  - I downloaded model from [here](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz)
  - I converted the model to an Intermediate Representation with the following arguments (here)[how-to-run.md]. 
  - The model was not very bad but with the small tweak in prob_thresold, it start to perform better.

  
- Model 2: TensorFlow Resnet v2
  - I downloaded model from [here](http://download.tensorflow.org/models/object_detection/ssd_resnet50_v1_fpn_shared_box_predictor_640x640_coco14_sync_2018_07_03.tar.gz)
  - I converted the model to an Intermediate Representation with the following arguments... [here](how-to-run.md)
  - The model was insufficient for the app because... the result it has very less accuracy, and current count of person in frame become zero if person is still in the frame. And this model is very slow. 

- Model 3: [Person detection retail]
  - I downloaded model from [here](https://docs.openvinotoolkit.org/latest/_models_intel_person_detection_retail_0013_description_person_detection_retail_0013.html)
  - I converted the model to an Intermediate Representation with the following arguments... not needed in this case.
  - The model was insufficient for the app because... This is the best model out of all three. It provided the best accuracy and is fastest.  
