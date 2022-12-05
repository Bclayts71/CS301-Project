In this branch is the google colab .ipynb file containing the code for semantic segmentation with model compression.

Explanation of code:

Similar to the semantic segmentation and hyperparameter optimization codes, downloading and resizing of images is the same. The same masks and images are used, as well as the same method of resizing the images to 256 by 256 pixels.

Model Compression Method:

The first method used to compress the model is pruning. The pruning algorithm works by finding variables or classifiers that are not needed for the training or prediction. For the variables that are less significant, a low weight is assigned to it. For the variables that are found to not be useful at all and maybe even cause overfitting, the weight is assigned to zero and the variable is non-affective. The pruning is applied after the initial training of the model, this way the accuracy can hopefully increase, and overfitting will be reduced. The second method that is used after pruning is called clustering. Clustering is similar to pruning in the way that it reduces the number of variables. However, clustering is done by comparing classifiers and combining them if they are similar. Clustering variables together has the same effects of hopefully reducing overfitting and improving the accuracy.

Model Compression Code Implementation:

Since I am compressing the completed model instead of reducing the model while it trains, I first needed a trained model. I used the same parameters to train the model as the first semantic segmentation code. Next, I import the model optimization api from TensorFlow to get the pruning and clustering methods. I use the polynomial decay method, and set my initial sparsity to 0 and final sparsity to 0.6. Also, I set the beginning step to 0 and end step to 100 because I want to use 100 epochs in my training. I compile the prune low magnitude model and then train it for 100 epochs. After pruning is done I strip the unnecessary parameters that pruning adds. My clustering parameters are: number of clusters = 6, preserve sparsity is true because I want the pruning effect to stay, and cluster centroids is set to density based which means the clustering is based on the weights. Then the sparsity clustered model is compiled and fitted with 50 epochs.

Results Obtained:

My first metric for analyzing the success of the training is the predicted mask. 
![image](https://user-images.githubusercontent.com/69495267/205551671-7c9dd3a8-7b7b-489d-8b7b-f943c60bcd6f.png)
![image](https://user-images.githubusercontent.com/69495267/205551676-c2485baa-66de-41bc-a452-695e24962fc7.png)
![image](https://user-images.githubusercontent.com/69495267/205551681-45abfd67-7269-4ff2-aed1-e06fa5dfcfb1.png)
![image](https://user-images.githubusercontent.com/69495267/205551700-34830edc-61bd-4ca0-8091-b6edaea99bd8.png)
![image](https://user-images.githubusercontent.com/69495267/205551707-3073946c-7eb3-4060-ace3-336ef739abf3.png)
![image](https://user-images.githubusercontent.com/69495267/205551710-b8dc9c5c-76ef-49d4-a05e-39a6d465a9ed.png)
![image](https://user-images.githubusercontent.com/69495267/205551727-53676254-6449-4891-aada-aaac77fd43f6.png)
![image](https://user-images.githubusercontent.com/69495267/205551734-ac25a1da-fc98-490e-bc15-0bb7e518b3e5.png)
![image](https://user-images.githubusercontent.com/69495267/205551740-cb991a61-ce04-4858-ad71-8511e8877504.png)
![image](https://user-images.githubusercontent.com/69495267/205551749-d1dc5edc-4757-48f3-b6d1-900cf1242d88.png)

The images above are probably the best images obtained from the three different methods of semantic segmentation used. Model compression definitely seems to have successfully helped overfitting because there are less cases of random colors popping up in different places. The predicted images could definitely be distinguished to be the respective mask, so I would consider this method a success.

My second metric for analyzing the success of the training is the training and validation loss graph. 

The image below is from the first training of the model with no compression.

![index](https://user-images.githubusercontent.com/69495267/205552276-d92d5eb6-805e-48dd-a6a7-b526d56c451c.png)

The image below is from the third training of the model with both methods of compression.

![index3](https://user-images.githubusercontent.com/69495267/205552300-c73f1ca9-d818-4b7f-b48b-b2f52ec9bf31.png)

The two differences I can see between the loss graphs are less overfitting in the compressed model, and lower loss. The lower loss is of course caused by the continuation of training from a previous model. However, the loss was still able to get pretty low thanks to the model compression. The reduction of overfitting can be seen by how much closer the loss and validation loss are to each other in the compressed model. In the model without compression, the loss is usually a lot less than the validation loss which is a sign of overfitting.

My third metric for analyzing the success of the training is the precision vs. recall graph.

The image below is from the first training of the model with no compression.

![index2](https://user-images.githubusercontent.com/69495267/205552473-8582a4c3-fc3b-4c35-9d13-15d7ea0802fa.png)

The image below is from the third training of the model with both methods of compression.

![index4](https://user-images.githubusercontent.com/69495267/205552481-3abb2b89-6b15-4e47-9940-33e150596d39.png)

The difference between the two precision recall graphs are lower precision and recall values in the first model, and the compressed model was more linear. In both cases it seems like the model successfully balanced the trade off of allowing more values to get through (recall) while still maintaining a high accuracy (precision).
