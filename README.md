# Transfer Learning for Edge Devices
 Create your own features using custom data and pre-trained models.  
 Utilizing google colab notebook we take an existing model and re-train to detect a custom object.  
 
 <img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/Jetson%20Nano%20realtime1.gif' width="440" height="260"><img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/450px-Nvidia-Jetson-Nano-2GB-Dev-Kit-1123x870.jpg' width="240" height="240">
 
 # Introduction 
 
 Creating deep learning models from scratch can be time consuming and not the best use of your time.  We are fortunate to have many pre-trained models available that we can use as a starting point for our own applications.  We can use transfer learning to re-train only a small portion that we need in order to develop our feature or solve our problem.
 
 <img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/from-colab-to-jetson%20(1).png' width="600" height="180">

For this project we will be re-training a base Model SSD-mobilenet-v1 pre-trained on the PASCAL
VOC dataset for object detection. We will download data from Google Open Images to retrain the
model to detect a custom object class (vehicle license plate). We will then convert and deploy the model
onto an Nvidia Jetson Nano.
 
 # Download dataset for training
Our base model can detect 20 objects out of the box but we want to re-train it to detect license plates.
We will be using Google Open Images to download our dataset. We have a choice of 600 different
classes but we will be using ‘Vehicle registration plate’ for our model. Using the website’s provided
script, we end up downloading 6800 images conveniently split up into test/train/validation. This
process can be done with any number of classes but we will be using one class for our example.

 
 <img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/TL%20image%20dl%20script.PNG' width="400" height="200"><img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/TL%20openimage%20traintestsplit.PNG' width="120" height="240">

The images themselves vary quite a bit in terms of viewing depth/angles and license plate types. We
will be trying to detect USA license plates but our dataset includes plates from all over the world.

<img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/open%20images%20sample.PNG' width="520" height="280">

Another big limitation of this dataset is its high variance in viewing angles, distances, cars and plate types.
Unfortunately, many of the highly curated datasets related to this particular object class are
proprietary and hard to find. 

# Re-training existing model
For this project we first re-trained on the Jetson Nano 2gb using the provided Jetson Utitlities script
with 100 epochs.

The 2Gb Nano is not very powerful for training and as a result takes almost 5 days to train. A faster
way to do this is by using Google Colab or your own higher powered GPU. We trained 100 epochs in
6.7 Hours using Google Colab on an Nvidia T4. The average time per epoch on the Nano was 86.
mins while Google Colab took 4 mins per epoch.

<img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/LT%20train.PNG' width="380" height="200">

_At the end of our training we achieved a loss of 2.78._

# Model Evaluation
 <img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/TL%20ssd%20Eval.PNG' width="500" height="50">
 <img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/LT%20avg%20precision.PNG' width="280" height="100">
The next step is to evaluate our model against some test data. Fortunately, during our initial
download of our Open Images dataset we created a test folder we can use to evaluate. The test
folder has 1109 Images. Running our evaluation script, we achieve a mAP of .569. Getting more than half recognition is ok performance but again the test set pictures are quite variable.  Some pre-trained license plate detection models can have mAP of over 90%.
The next step is to convert this model and deploy it to our Jetson Nano.
 
 
 # Export and Deploy to Edge Device
 
 <img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/TL%20onnx%20export.PNG' width="360" height="180">
 
 After re-training our model we need to convert it to an .onnx file to load onto the Jetson Nano.  
ONNX is an open model format that supports many of the popular ML frameworks, including PyTorch, TensorFlow, TensorRT, and others, so it simplifies transferring models between tools.  
We can use the onnx_export.py script provided by jetson utilities.  Then with TensorRT (pre-loaded onto the nano) we can optimize the model for deployment on the Jetson Nano.

Running in real-time on the Jetson Nano we get a consistent 30fps.  
<img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/Jetson%20Nano%20realtime.gif' width="440" height="260">
<img src='https://github.com/particleman14/Transfer-Learning-Application/blob/main/Files/Jetson%20Nano%20realtime1.gif' width="440" height="260">

In these clips, we ran a youtube video and pointed a web-camera at the screen for real-time detection.
Overall the device captured most vehicles that were on screen, the caveat being the viewing angles
were very tight. Viewing angles beyond 30* don’t seem to get detected.
  
# Potential Applications

We are only detecting one object in this example to save training time and to deploy on a low end
edge device. Even with a Jetson Nano (entry level compute product) we can get good real-time
detection. We could potentially expand this to detect several objects of our choosing like pose
estimation or facial recognition. Perhaps we can make a smart doorbell or a room monitor.

Some other potential applications include medical imaging and sensing, smarter NLP and chatbots,
computer vision problems, forecasting specific events.

# Pros and Cons of Transfer Learning Method

__The Pros of Transfer Learning__
* Increase performance vs. starting from scratch
* Saves time
* Doesn’t necessarily need massive amounts of data

__The Cons of Transfer Learning__
* Need model that’s somewhat related to your task, sometimes they don’t exist
* Risk of negative transfer where model ends up performing worse


# Conclusion
The new paradigm of transfer learning allows for smaller organizations and individuals to
create their specific features for their specific application. This is more akin to learning from previous
generations and building upon that collective knowledge. Using pre-trained models saves us time
and energy to try and develop our own unique features and uses.

Transfer learning is popular because of its wide range of applications but not all problems are
suited for this type of method. Identifying the problem beforehand will greatly aid you in figuring out
the best method to deploy. Overall, transfer learning techniques are here to stay and will continue to
be an important part of AI and Machine Learning applications in the future.








