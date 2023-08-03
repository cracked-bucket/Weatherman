# Weatherman

A program that uses a jetson nano and a webcam that's capable of viewing and classifying various weather patterns via imagenet and a pre-trained network.

![A Foggy Day](https://wpcdn.us-east-1.vip.tn-cloud.net/www.wmdt.com/content/uploads/2021/01/fog-1.jpg)

##The Algorithm

Images were all taken from this dataset: https://www.kaggle.com/datasets/jehanbhathena/weather-dataset Resnet will store all the files within 18 layers of classification. When a command is ran to classify an image, imagenet will parse through the command looking for keywords. It will then load the recognition network, and then scans through the image, capturing each individual frame and processing it through its algorithm. Once it does this, the program will classify the image, determines the appropriate class labels from labels.txt to input into the the title, renders the image, then updates the title bar and status to match the classification progress to the final result. Once the classification is completed, the program will exit out of the system.

##Running This Project

Assuming you have a jetson nano connected to wifi as well as a webcam, follow these steps:

1. Download the kaggle dataset listed and create a testing, training, and validation folder for each weather pattern
2. Create a folder called weather in jetson-inference/python/training/classification/data
3. Input the dataset folders you just created
4. Run the docker in the terminal via ./docker/run.sh
5. Go into jetson-inference/python/training/classification
6. Retrain the network in the models directory via python3 train.py --model-dir=models/weather data/weather for as many epochs as you want.
7. Export to onnx via python3 onnx_export.py --model-dir=models/weather
8. To process it, press CTRL+D to exit the docker
9. Set the NET variable: NET=models/weather
10. Set the DATASET variable: DATASET=data/weather
11. imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/(EXAMPLE)/01.jpg (EXAMPLE).jpg
12. To use the camera, run imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt (CAMERA)
