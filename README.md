# `Mask-and-Non-mask-face-detection`
<br>
<strong>A deep learning model to detect mask and non mask wearing people.</strong>
<br>
<br>
We all are dealing with global pandemic.In such situation,Mask is prove to be the best defender from virus.<br>
Detection of mask will help community to monitor mask and non mask wearing people in real time with high level deep learning model.<br>
I had made a mask and non mask wearing face detection model.<br>
The model is build over the wonderful <strong>keras implementation of retinanet object detection developed by <a  href="https://github.com/fizyr/keras-retinanet1">Fiyzr</a></strong>
<br>
<br>
<h2>Retinanet Overview</h2>
<br>
RetinaNet is a single, unified network composed of a backbone network and two task-specific subnetworks. The backbone is responsible for computing a conv feature map over an entire input image and is an off-the-self convolution network. The first subnet performs classification on the backbones output; the second subnet performs convolution bounding box regression.
<br>
<strong>Backbone:</strong> Feature Pyramid network built on top of ResNet50 or ResNet101.
<br>
<strong>Classification subnet:</strong> It predicts the probability of object presence at each spatial position for each of the A anchors and K object classes. Takes a input feature map with C channels from a pyramid level, the subnet applies four 3x3 conv layers, each with C filters amd each followed by ReLU activations. Finally sigmoid activations are attached to the outputs. Focal loss is applied as the loss function.
<br>
<strong>Box Regression Subnet:</strong> Similar to classification net used but the parameters are not shared. Outputs the object location with respect to anchor box if an object exists. smooth_l1_loss with sigma equal to 3 is applied as the loss function to this part of the sub-network.
<br>
To know more about model working view <a href="https://medium.com/@14prakash/the-intuition-behind-retinanet-eb636755607d">here.</a>
<br>
<br>
<h2>Detection Model</h2>
The Data for mask and non mask wearing faces was not available anywhere.I made the dataset which you can download from <a href="https://drive.google.com/file/d/1Iz4rYUWJRXY_UG0VDwMpRAG2Oi8LrvKl/view?usp=sharing">here</a>
<br>
<h3>Steps to develop model:</h3>
Make sure to have image dataset.<br>
The images need to be labeled which can be done by using <a href="https://github.com/tzutalin/labelImg">Labelimg</a><br>
Make sure to fulfil the input requirements for the model.<br>
Run the model by changing the require fields as given in usage.
<br>
<br>
<h2>Usage:</h2>
<h4>1. Create annotations using tool labelimg which create annotations in pascalvoc format.</h4>

<h4>2. Convert annotations into fiyzr format by</h4>
create a zip file containing training dataset images and annotations with the same filename <br>
Upload zip file in Google Drive, get Drive file id, and substitute the DATASET_DRIVEID value.<br>
Run cell that iterates over the xml files and creates annotations.csv file
  
<h4>3. Model training:</h4>
Fizyr offers various parameters, described in <a href="https://github.com/fizyr/keras-          retinanet/blob/c841da27f540084d27e971b6d00c178ff005d344/keras_retinanet/bin/train.py#L358">Github</a>, to run and optimize this step.

It’s a good option to start from a pretrained model instead of training a model from scratch. Fizyr released a model based on     ResNet50 architecture, pretrained on Coco dataset.I had used the same for this model.You can download it from <a href="https://github.com/fizyr/keras-retinanet/releases">here.</a><br>
<strong>Train your dataset with</strong>
<br>
 `!keras_retinanet/bin/train.py --freeze-backbone --random-transform --weights {PRETRAINED_MODEL} --batch-size 8 --steps 500 --epochs 25 csv annotations.csv classes.csv`
<br>
<h6>Let’s analyze each argument passed to the script.</h6>
<strong>freeze-backbone:</strong> freeze the backbone layers, particularly useful when we use a small dataset, to avoid overfitting<br>
<strong>random-transform:</strong> randomly transform the dataset to get data augmentation<br>
<strong>weights:</strong> initialize the model with a pretrained model (your own model or one released by Fizyr)<br>
<strong>batch-size:</strong> training batch size, higher value gives smoother learning curve<br>
<strong>steps:</strong> number of steps for epochs<br>
<strong>epochs:</strong> number of epochs to train<br>
<strong>csv:</strong> annotations files generated by the script above
     
<h4>4. Inference</h4>
The last step performs inference of test images with the trained model.
The Fizyr framework allows us to perform inference using CPU, even if you trained the model with GPU. This feature is important in      typical production environments, where people usually opt for less expensive hardware infrastructures for inference, without GPUs.<br>
<strong>Let's analyze</strong>

`model_path = os.path.join('snapshots', sorted(os.listdir('snapshots'), reverse=True)[0])print(model_path)`

`# load retinanet modelmodel = models.load_model(model_path, backbone_name='resnet50')`

`model = models.convert_model(model)`

The first line sets the model file as the last model generated by the training process in /snapshots directory. Then the model is        loaded from the filesystem and converted to run inference.
You can change the values of THRES_SCORE, which represents the confidence threshold to show an object detection.
 
<h4>5. Test</h4>
Test your model by uploading various different images in the following funtion:
<br>
 `img_inference(img_infer)`
 <br>
<strong>The output from this model is:</strong>
<br>
    <img width="250" height="300" src="https://github.com/nehasm/Mask-and-Non-mask-face-detection/blob/master/output/output1.PNG" alt="darkbackground" border="0">
 <br>
    <img width="250" height="300" src="https://github.com/nehasm/Mask-and-Non-mask-face-detection/blob/master/output/output2.PNG" alt="darkbackground" border="0">

<h4>6. Check your model in real time data</h4>
For the futher testing of model,the video is passed into model to detect the classes.
Pass the correct video_path and output_path as shown in repository.<br>
<strong>Get the output by using following funtion:</strong>

`run_detection_video(video_path)`

To check the output from this model <a href="https://drive.google.com/file/d/1oNG2glGKtfNl-2mXL-e4Y4TEABMDbSbY/view?usp=sharing">Click here</a>
Hence,this was the complete implementation of object detection model Retinanet from fiyzr for mask and non mask wearing face detction. 
   

     
     





