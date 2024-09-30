▎Sign Language Translator

A sign language translator is a tool or system designed to interpret and translate sign language into spoken or written language, and vice versa. It aims to bridge communication gaps between deaf or hard-of-hearing individuals and those who do not know sign language. By using various technologies, such as computer vision and machine learning, these translators can recognize hand gestures and movements, converting them into text or speech. This fosters inclusivity and enhances communication in diverse environments, making information accessible to everyone.

▎Project Path Report

Initially, we use a detector for hand recognition, and then we classify various hand movements, each representing letters of the sign language alphabet. The project consists of two scripts: the first script is for hand detection, cropping, and collecting images for training, while the second script is for testing the data. Using cvzone, we detect both right and left hands along with their skeletons and joints. 

In the main image, y represents the starting height, and h+y represents the ending height; x is the starting width, and w+x is the ending width, which gives us the bounding box. To better display the hands, I considered an offset of 20. When we want to classify the data, we need to crop the images into square shapes of equal size. The height and width of each image differ to represent different letters, and a significant amount of information may be lost after cropping. As a solution, I create a matrix of ones using np, resulting in a white square of 300×300 named imgwhite with a data type of uint8. We place each image we have in the center of this square. Here, mathematics is crucial, as even a 1-pixel error can lead to inaccuracies. We check if the height is greater than the width; if so, we set the height to 300 and calculate how much we need to stretch the width to maintain the correct proportions, placing the image in the center of the square.

▎Width Calculation

First, we calculate w/h, then create an if statement: if the output is greater than 1, it means the height is greater; otherwise, the width is greater. If the height is greater, we divide the image size by height to get k (a constant). Using ceil, when calculating the output width size, we round k × width to the nearest whole number. For example, if the output is 4.5, we consider it as 5. Finally, we place the cropped image (with calculated width wcal and height 300) as the resized image. To center the resized image, we need to divide (image - wcal) / 2 to create a gap called wgap for the width. In imgwhite, the height is 300; wgap is at the starting width and wcal + wgap at the ending width.

▎Length Calculation

If the width is greater, we divide the image size by width to get k (a constant). Using ceil, when calculating the output length size, we round k × height to the nearest whole number. For example, if the output is 4.5, we consider it as 5. Finally, we place the cropped image (with calculated height hcal and width 300) as the resized image. To center this resized image, we divide (image - hcal) / 2 to create a gap called hgap for length. In imgwhite, hgap is at the starting height and hgap + hcal at the ending height; width remains 300.

▎Saving Images

Now it's time to save images for the dataset. We define a key named 's' that saves images in the desired folder when pressed. We have a counter that indicates how many images are taken for each letter; here, approximately 700 images are captured for each letter to ensure more accurate performance after training.

▎Training with Google Teachable Machine

After collecting images, we train them with Google Teachable Machine, then download the trained Keras model. This gives us a zip file that we save in a new folder named "model" and then import in the test file. We need to have TensorFlow installed; in the test file, we copy some of the code from the data collection file, comment out certain lines, and import the classifier from cvzone. We provide the paths to the Keras files and labels in the classifier.

To get the index and prediction of an image, we add code for this purpose. Since we don't want to display skeletons and joints, we set false=draw, and instead show a purple rectangle around the hand along with its label.


