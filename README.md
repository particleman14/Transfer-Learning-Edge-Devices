# Transfer Learning for Edge Devices

 Create your own features using custom data and pre-trained models.  
 
 Utilizing google colab notebook we take an existing model and re-train to detect a custom object.  
 
 __Introduction__
 
 Creating deep learning models from scratch can be time consuming and not the best use of your time.  We are fortunate to have many pre-trained models available that we can use as a starting point for our own applications.  We can use transfer learning to re-train only a small portion that we need in order to develop our feature or solve our problem.
 
 
 __Train model with Custom Data__
 
 Our base model can detect 20 objects out of the box but we want to re-train it to detect license plates.  We will be using Google Open Images to download our dataset.  We have a choice of 600 different classes but we will be using ‘Vehicle registration plate’ for our model.  Using the website’s provided script, we end up downloading 6800 images conveniently split up into test/train/validation.  This process can be done with any number of classes but we will be using one class for our example.
 
 
 __Export and Deploy to Edge Device__
 
 After re-training our model we need to convert it to an .onnx file to load onto the Jetson Nano.  
ONNX is an open model format that supports many of the popular ML frameworks, including PyTorch, TensorFlow, TensorRT, and others, so it simplifies transferring models between tools.  
We can use the onnx_export.py script provided by jetson utilities.  Then with TensorRT (pre-loaded onto the nano) we can optimize the model for deployment on the Jetson Nano.


  
 __Performance and Potential Applications__

Running in real-time on the Jetson Nano we get a consistent 30fps.  












