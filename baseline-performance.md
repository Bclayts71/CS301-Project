In this branch is the google colab .ipynb file containing the code for the first segmentation models.

Explanation of Code:

To begin the program I start by setting up the environment. This includes cloning the github repo python_for_microscopists
and installing my necessary dependencies. These dependencies are nni, kaggle, and segmentation_models. The last thing I did
to set up my environment was getting the semantic-segmentation-of-aerial-imagery dataset from kaggle and extracting the files.


Now that the environment was set up, I had to preprocess the data so it could be trained properly. The images and masks came in
all different sizes, so I had to resize each image to 256 by 256. I walked through each file in the dataset, and if the file was
an image ending in .jpg, then I cropped the image to 256 by 256 and resized it divided by 255. I did mostly the same thing for
if the file was a mask ending in .png except I didn't divide the resize by 255 and I converted the BGR to RGB.

The next step after resizing the data was to get the color scheme correct. The hexadecimal codes were provided as follows: building = '3C1098', land = '8429F6', 
road = '6EC1E4', vegetation = 'FEDD3A', water = 'E2A929', and unlabeled = '9B9B9B'. First, I had to convert the hexadecimal to RGB by taking each
set of 2 numbers and converting it to decimal. Next, I iterated through all of the pixel values of the masks and changed the RGB to an integer value
for easy classification. Lastly, I convert those integer values to one hot encoded, so that the machine can classify and categorize the colors.

The data is finally preprocessed and ready to be trained. Testing and training data sets are created by splitting the image data set as well as the
one hot encoded data set. The loss function is calculated by adding the dice loss, which is the pixel overlap between the predicted and ground truth
and the focal loss, which is essentially just cross-entropy loss. The final variables I added for the model are the metrics: accuracy, jacard_coef,
precision, and recall. The fitting of the dataset is based on the UNet model. The first stage of UNet is the convolution process where the image is split, so
that the size of the image is reduced and the depth of the image is increased. The process is repeated until UNet hits the bottom convolution. From here
the image begins growing again by using transposed convolution. Transposed convolution is basically just a method for growing the image. This image is
concatenated with the corresponding image from the first process. This process is repeated to get a more precise prediction, until the image reaches the
upper convolution. Lastly, the image is put back together to the size requirement and returned for the first epoch. For my model in the google colab
file attached, I used 200 epochs.

Once the model has finished training I create 10 images showing the difference between the testing image, testing mask, and the mask that my model predicted.
Also, I created 2 graphs which are the loss and validation loss at each epoc, and the precision vs recall.

Results Obtained:

My first metric for analyzing the success of the training is the predicted mask.
![image](https://user-images.githubusercontent.com/69495267/200220925-5d61c862-3206-4082-9a0e-8af8dcd07cc0.png)
![image](https://user-images.githubusercontent.com/69495267/200220944-efd50057-3648-4a59-a280-a655b091e24e.png)
![image](https://user-images.githubusercontent.com/69495267/200220957-4de96436-f2d1-4a54-8890-fcd1eaec04b9.png)
![image](https://user-images.githubusercontent.com/69495267/200220968-354b3a6f-0023-4c09-ab81-d29436ab7da6.png)
![image](https://user-images.githubusercontent.com/69495267/200220973-faf9e110-3f16-4710-a211-c3a3380922da.png)
![image](https://user-images.githubusercontent.com/69495267/200220977-df4a8f4e-6907-4a68-b344-d663d33b5197.png)
![image](https://user-images.githubusercontent.com/69495267/200220983-426511f4-094b-49e7-8e6b-9cd425d97cc5.png)
![image](https://user-images.githubusercontent.com/69495267/200220992-603b3a9c-717d-4469-96b2-ff7e400d9832.png)
![image](https://user-images.githubusercontent.com/69495267/200221002-99d79c96-c523-4361-9ede-37ae4aed2dd8.png)
![image](https://user-images.githubusercontent.com/69495267/200221010-5144be4b-2457-44f3-ac04-53f49e1c0ff2.png)

The first image is the test image, the second image is the testing mask, and the third image is my produced prediction. Looking at the differences between
the predicted and testing label, it is clear that the model did a pretty good job. All of the pictures closely resemble the test images and only miss
out on minute details. For example, in image set 2, the test image missed the road. The road is very covered by the sand, so since the prediction got the
rest of the image correct I would consider it a success.

My second metric for analyzing the success of the training is the training and validation loss graph.
![image](https://user-images.githubusercontent.com/69495267/200436061-9f0a1210-4f65-4800-9392-381e3df06c10.png)

The graph clearly depicts that as the number of epochs increases, the loss decreases. This is a very good sign because it shows that the model
is actually training, and that the more the model trains itself, the better it gets at predicting.

My third metric for analyzing the success of the training is the precision vs. recall graph.

![image](https://user-images.githubusercontent.com/69495267/200436106-b785c6f2-55c7-4fef-8fe8-97d795ba9581.png)

In the graph, the precision and recall both start very low which is most likely caused by the early inaccuracy of the model. However, the precision
increases very quickly. Then as recall starts to increase, the precision decreases at certain points. This is a balancing act of trying to recall as
many points as possible while also being precise. On the right of the graph it is seen that the precision and recall balance each other out for a very
accurate model.
